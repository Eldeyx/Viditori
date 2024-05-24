<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Viditori - Red Social</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }

        header {
            background-color: #333;
            color: white;
            padding: 10px;
            text-align: center;
        }

        main {
            padding: 20px;
        }

        #mediaContainer {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            grid-gap: 20px;
        }

        .media {
            position: relative;
            border-radius: 5px;
            overflow: hidden;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 100%;
        }

        .media video,
        .media img {
            width: 100%;
            height: auto;
            max-height: 300px;
        }

        .commentsSection {
            background-color: rgba(255, 255, 255, 0.9);
            padding: 10px;
            box-sizing: border-box;
            display: none;
        }

        .commentsList {
            margin-top: 10px;
            max-height: 100px;
            overflow-y: auto;
        }

        .commentInput {
            width: calc(100% - 20px);
            margin-top: 5px;
        }

        .commentButton,
        .sendButton {
            width: calc(50% - 20px);
            margin-top: 5px;
            padding: 10px;
            background-color: #333;
            color: white;
            border: none;
            cursor: pointer;
        }

        .commentForm {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .commentForm textarea {
            flex: 1;
            margin-right: 10px;
        }

        .commentButton {
            margin-top: 10px;
            padding: 10px;
            background-color: #333;
            color: white;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <header>
        <h1>Bienvenida a Viditori - Red Social</h1>
    </header>

    <main>
        <div id="mediaContainer">
            <!-- Aquí se mostrarán los videos e imágenes -->
        </div>

        <div id="uploadForm">
            <input type="file" id="mediaFile" accept="video/*, image/*">
            <button onclick="subirMedia()">Subir Video o Imagen</button>
        </div>
    </main>

    <script>
        // Objeto para almacenar los comentarios de cada video
        var comentariosPorVideo = {};

        function subirMedia() {
            var input = document.getElementById('mediaFile');
            var file = input.files[0];
            
            if (file) {
                var reader = new FileReader();
                
                reader.onload = function(e) {
                    var mediaSrc = e.target.result;
                    var mediaContainer = document.getElementById('mediaContainer');
                    var mediaElement = document.createElement('div');
                    mediaElement.classList.add('media');
                    
                    if (file.type.startsWith('video')) {
                        mediaElement.innerHTML = `
                            <video controls>
                                <source src="${mediaSrc}" type="${file.type}">
                            </video>
                            <button class="commentButton" onclick="mostrarComentarios('${mediaSrc}')">Comentarios</button>
                            <div class="commentsSection" id="comments-${mediaSrc}">
                                <div class="commentsList"></div>
                                <textarea class="commentInput" placeholder="Escribe un comentario"></textarea>
                                <button class="sendButton" onclick="agregarComentario('${mediaSrc}')">Enviar</button>
                            </div>
                        `;
                    } else if (file.type.startsWith('image')) {
                        mediaElement.innerHTML = `
                            <img src="${mediaSrc}" alt="Imagen">
                            <button class="commentButton" onclick="mostrarComentarios('${mediaSrc}')">Comentarios</button>
                            <div class="commentsSection" id="comments-${mediaSrc}">
                                <div class="commentsList"></div>
                                <textarea class="commentInput" placeholder="Escribe un comentario"></textarea>
                                <button class="sendButton" onclick="agregarComentario('${mediaSrc}')">Enviar</button>
                            </div>
                        `;
                    }

                    mediaContainer.appendChild(mediaElement);

                    // Crear objeto de comentarios para este video
                    comentariosPorVideo[mediaSrc] = [];
                };
                
                reader.readAsDataURL(file);
            } else {
                alert('Selecciona un archivo primero');
            }
        }

        function mostrarComentarios(mediaSrc) {
            var commentsSection = document.getElementById(`comments-${mediaSrc}`);
            commentsSection.style.display = 'block';
            var commentsList = commentsSection.querySelector('.commentsList');
            commentsList.innerHTML = '';
            var comentarios = comentariosPorVideo[mediaSrc];
            comentarios.forEach(function(comentario) {
                var commentItem = document.createElement('div');
                commentItem.textContent = comentario;
                commentsList.appendChild(commentItem);
            });
        }

        function agregarComentario(mediaSrc) {
            var commentsSection = document.getElementById(`comments-${mediaSrc}`);
            var commentInput = commentsSection.querySelector('.commentInput');
            var comentario = commentInput.value.trim();
            if (comentario !== '') {
                comentariosPorVideo[mediaSrc].push(comentario);
                commentInput.value = '';
                mostrarComentarios(mediaSrc);
            } else {
                alert('Por favor escribe un comentario antes de enviar.');
            }
        }
    </script>
</body>
</html>
