<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Subir y Mostrar Videos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
        }

        #fileInput {
            display: none;
        }

        #fileLabel {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }

        #videoList {
            margin-top: 20px;
            white-space: nowrap;
            overflow-x: auto;
        }

        .video-container {
            display: inline-block;
            vertical-align: top;
            margin-right: 10px;
        }

        .delete-button {
            cursor: pointer;
            color: red;
        }
    </style>
</head>
<body>
    <h1>Álbum de videos❤️</h1>

    <!-- Input para seleccionar archivos -->
    <input type="file" id="fileInput" accept="video/*" multiple>
    <label for="fileInput" id="fileLabel">Seleccionar Videos</label>

    <!-- Lista para mostrar archivos subidos -->
    <div id="videoList"></div>

    <!-- Botón para eliminar videos y reiniciar -->
    <button onclick="clearAndReload()">Eliminar Videos y Reiniciar</button>

    <script>
        function handleFileSelect(event) {
            const fileList = event.target.files;
            const videoList = document.getElementById('videoList');

            for (const file of fileList) {
                if (file.type.startsWith('video/')) {
                    const videoContainer = document.createElement('div');
                    videoContainer.className = 'video-container';

                    const video = document.createElement('video');
                    video.src = URL.createObjectURL(file);
                    video.controls = true;

                    const deleteButton = document.createElement('span');
                    deleteButton.className = 'delete-button';
                    deleteButton.textContent = 'Eliminar';
                    deleteButton.addEventListener('click', () => deleteVideo(videoContainer));

                    videoContainer.appendChild(video);
                    videoContainer.appendChild(deleteButton);
                    videoList.appendChild(videoContainer);
                }
            }

            updateLocalStorage();
        }

        function deleteVideo(videoContainer) {
            const confirmDelete = confirm('¿Estás seguro de que deseas eliminar este video?');

            if (confirmDelete) {
                videoContainer.remove();
                updateLocalStorage();
            }
        }

        function updateLocalStorage() {
            const videoList = document.getElementById('videoList');
            localStorage.setItem('videoList', videoList.innerHTML);
        }

        const fileInput = document.getElementById('fileInput');
        const fileLabel = document.getElementById('fileLabel');
        const videoList = document.getElementById('videoList');

        if (localStorage.getItem('videoList')) {
            videoList.innerHTML = localStorage.getItem('videoList');
        }

        fileInput.addEventListener('change', handleFileSelect);

        fileInput.addEventListener('change', function () {
            fileLabel.textContent = fileInput.files.length > 1
                ? `${fileInput.files.length} Archivos Seleccionados`
                : fileInput.files[0].name;
        });

        // Función para eliminar videos y reiniciar la página
        function clearAndReload() {
            localStorage.removeItem('videoList');
            videoList.innerHTML = '';
        }
    </script>
</body>
</html>
