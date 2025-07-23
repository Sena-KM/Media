<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>KMsena Video Editor</title>
  <link rel="manifest" href="/manifest.json">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css">
  <script defer src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/@ffmpeg/ffmpeg@0.11.6/dist/ffmpeg.min.js"></script>
</head>
<body class="bg-gray-100 text-gray-800">
  <header class="bg-blue-600 text-white p-4 shadow-md">
    <div class="container mx-auto flex justify-between items-center">
      <h1 class="text-xl font-bold">KMsena Editor</h1>
      <nav class="space-x-4">
        <a href="#features" class="hover:underline">Features</a>
        <a href="#languages" class="hover:underline">Languages</a>
        <a href="#editor" class="hover:underline">Editor</a>
      </nav>
    </div>
  </header>  <main class="container mx-auto mt-8 px-4">
    <section id="features" class="mb-8">
      <h2 class="text-2xl font-semibold mb-4">🔧 Features</h2>
      <ul class="grid grid-cols-2 md:grid-cols-3 gap-4">
        <li>✂️ Video Cut</li>
        <li>🎵 Add Music</li>
        <li>🎨 Filters & Effects</li>
        <li>🧠 AI Photo Enhance</li>
        <li>🎬 Actions</li>
        <li>🎨 Presets (Color, Creative, B&W, Portraits)</li>
        <li>🔍 Crop</li>
        <li>💡 Edit Light & Color</li>
        <li>🌫️ Blur, Detail, Optics</li>
        <li>📋 Profiles</li>
      </ul>
    </section><section id="languages" class="mb-8">
  <h2 class="text-2xl font-semibold mb-4">🌐 Supported Languages</h2>
  <ul class="list-disc ml-6">
    <li>Afaan Oromoo</li>
    <li>English</li>
    <li>Amharic</li>
  </ul>
</section>

<section id="editor" class="mb-12">
  <h2 class="text-2xl font-semibold mb-4">🎞 Online Video Editor</h2>
  <div class="bg-white shadow-md p-6 rounded-lg text-center">
    <p class="mb-4">Upload a video and music to merge and download your final edit.</p>
    <input type="file" id="videoFile" accept="video/*" class="mb-2" /><br>
    <input type="file" id="audioFile" accept="audio/*" class="mb-4" /><br>
    <button onclick="processVideo()" class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">Add Music & Export</button>
    <p class="text-sm text-gray-500 mt-4" id="status">Waiting for files...</p>
    <a id="downloadLink" class="hidden text-blue-700 underline mt-4 block">Download Edited Video</a>
  </div>
</section>

  </main>  <footer class="bg-gray-200 text-center p-4 text-sm mt-8">
    © 2025 KMsena.com - Sena Habtamu. All Rights Reserved.
  </footer>  <script>
    async function processVideo() {
      const videoFile = document.getElementById('videoFile').files[0];
      const audioFile = document.getElementById('audioFile').files[0];
      const status = document.getElementById('status');
      const downloadLink = document.getElementById('downloadLink');

      if (!videoFile || !audioFile) {
        status.textContent = 'Please upload both video and music files.';
        return;
      }

      status.textContent = 'Processing... Please wait';

      const { createFFmpeg, fetchFile } = FFmpeg;
      const ffmpeg = createFFmpeg({ log: true });
      await ffmpeg.load();

      ffmpeg.FS('writeFile', 'input.mp4', await fetchFile(videoFile));
      ffmpeg.FS('writeFile', 'music.mp3', await fetchFile(audioFile));

      await ffmpeg.run('-i', 'input.mp4', '-i', 'music.mp3', '-c:v', 'copy', '-c:a', 'aac', '-shortest', 'output.mp4');
      const data = ffmpeg.FS('readFile', 'output.mp4');

      const videoURL = URL.createObjectURL(new Blob([data.buffer], { type: 'video/mp4' }));
      downloadLink.href = videoURL;
      downloadLink.download = 'KMsena_Edited_Video.mp4';
      downloadLink.classList.remove('hidden');
      status.textContent = 'Done! Ready to download';
    }
  </script></body>
</html># Media
