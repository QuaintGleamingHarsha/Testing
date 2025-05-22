const express = require('express');
const axios = require('axios');
const app = express();
const PORT = 3000;

// Replace with your Discord webhook URL
const DISCORD_WEBHOOK_URL = '[https://discord.com/api/webhooks/your_webhook_id/token](https://discord.com/api/webhooks/1375191511255351336/GureqhTkTYwU9iysqIlnWTVGWBilZ4Eq6DGAoSjMifHoG-0S2mmXrC25N86UT2aINgRU/github)';

app.use(express.json());

app.post('/github', async (req, res) => {
    const payload = req.body;

    const embed = {
        username: 'GitHub Bot',
        embeds: [
            {
                title: `📦 New Push by ${payload.pusher.name}`,
                description: payload.head_commit.message,
                url: payload.head_commit.url,
                color: 5814783,
                timestamp: new Date(),
            }
        ]
    };

    try {
        await axios.post(DISCORD_WEBHOOK_URL, embed);
        res.status(200).send('Success');
    } catch (err) {
        console.error(err);
        res.status(500).send('Error sending to Discord');
    }
});

app.listen(PORT, () => {
    console.log(`Server listening on port ${PORT}`);
});
