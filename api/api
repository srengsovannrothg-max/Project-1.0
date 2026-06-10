export default async function handler(req, res) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  if (req.method === 'OPTIONS') return res.status(200).end();
  if (req.method !== 'POST') return res.status(405).json({ error: 'Method not allowed' });

  const { provider, payload } = req.body;

  try {
    let apiUrl, apiKey, headers;

    if (provider === 'deepseek') {
      apiUrl = 'https://api.deepseek.com/v1/chat/completions';
      apiKey = process.env.DEEPSEEK_API_KEY;
    } else if (provider === 'openai') {
      apiUrl = 'https://api.openai.com/v1/chat/completions';
      apiKey = process.env.OPENAI_API_KEY;
    } else {
      return res.status(400).json({ error: 'Unknown provider. Use "deepseek" or "openai".' });
    }

    if (!apiKey) {
      return res.status(500).json({ error: `API key for ${provider} is not set in environment variables.` });
    }

    const upstream = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${apiKey}`
      },
      body: JSON.stringify(payload)
    });

    const data = await upstream.json();
    return res.status(upstream.status).json(data);

  } catch (err) {
    return res.status(500).json({ error: err.message });
  }
}
