in this application you can add your bucket list things and delete when you finish it

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Note Taking App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .note-container {
            display: flex;
            justify-content: space-between;
        }
        .note-list {
            width: 30%;
            border: 1px solid #ccc;
            padding: 20px;
        }
        .note-content {
            width: 65%;
            border: 1px solid #ccc;
            padding: 20px;
        }
        .note-item {
            margin-bottom: 10px;
        }
        .note-item:hover {
            cursor: pointer;
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>
    <div class="note-container">
        <div class="note-list">
            <h2>Notes</h2>
            <div id="note-list"></div>
            <button id="add-note">Add Note</button>
        </div>
        <div class="note-content">
            <h2 id="note-title">Note Title</h2>
            <textarea id="note-text" rows="10" cols="50"></textarea>
            <button id="save-note">Save Note</button>
        </div>
    </div>
    <script>
        let notes = JSON.parse(localStorage.getItem('notes')) || [];

        function displayNotes() {
            const noteList = document.getElementById('note-list');
            noteList.innerHTML = '';

            notes.forEach((note, index) => {
                const noteItem = document.createElement('div');
                noteItem.classList.add('note-item');
                noteItem.textContent = note.title;

                noteItem.addEventListener('click', () => {
                    document.getElementById('note-title').textContent = note.title;
                    document.getElementById('note-text').value = note.text;
                });

                noteList.appendChild(noteItem);
            });
        }

        document.getElementById('add-note').addEventListener('click', () => {
            const noteTitle = prompt('Enter note title:');
            if (!noteTitle) return;

            const noteText = prompt('Enter note text:');

            const newNote = {
                title: noteTitle,
                text: noteText
            };

            notes.push(newNote);
            localStorage.setItem('notes', JSON.stringify(notes));

            displayNotes();
        });

        document.getElementById('save-note').addEventListener('click', () => {
            const noteTitle = document.getElementById('note-title').textContent;
            const noteText = document.getElementById('note-text').value;

            const noteIndex = notes.findIndex(note => note.title === noteTitle);

            if (noteIndex > -1) {
                notes[noteIndex].text = noteText;
            } else {
                notes.push({ title: noteTitle, text: noteText });
            }

            localStorage.setItem('notes', JSON.stringify(notes));
        });

        displayNotes();
    </script>
</body>
</html>
