const express = require('express');
const axios = require('axios');
const app = express();
const port = process.env.PORT || 3000;

app.use(express.static('public'));

app.get('/download', async (req, res) => {
  const videoUrl = req.query.url;
  if (!videoUrl) return res.status(400).send('URL parameter is required');

  try {
    const response = await axios.get(videoUrl, { responseType: 'stream' });
    res.setHeader('Content-Disposition', 'attachment; filename="video.mp4"');
    response.data.pipe(res);
  } catch (err) {
    res.status(500).send('Error downloading video');
  }
});

app.listen(port, () => console.log(`Server running on port ${port}`));
