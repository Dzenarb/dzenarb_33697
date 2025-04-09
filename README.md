📌 Title: No Rate Limiting on Magic Link Email Endpoint – Potential Email Flooding (DoS)
🛡️ Severity: Medium (DoS vector, low complexity, affects user experience and email service reputation)
🔍 Summary:
The magic-link endpoint of the lfd-api.tanssi.network domain allows unlimited POST requests with the same email address, causing a flood of authentication emails without any rate-limiting, CAPTCHA, or throttling mechanisms. This can be abused for email bombing attacks, degrading the user experience and harming the platform's email deliverability/reputation.

📎 Endpoint:
perl
Копировать
Редактировать
POST https://lfd-api.tanssi.network/api/auth/magic-link
🧪 Steps to Reproduce:
Open terminal or any scripting tool (e.g. bash).

Run the following script:

bash
Копировать
Редактировать
for i in {1..20}; do
  curl 'https://lfd-api.tanssi.network/api/auth/magic-link' \
    -H 'Content-Type: application/json' \
    -H 'Origin: https://lfd.tanssi.network' \
    --data-raw '{"email":"victim@example.com"}'
  sleep 1
done
Observe that all requests return 200 OK.

Check the inbox of the victim email address — they receive 20+ emails instantly.

⚠️ Impact:
📬 Victim's inbox can be flooded, leading to annoyance or denial of legitimate emails.

📉 Damages sender domain's reputation (spam score, blacklisting).

🚫 No visible limit, CAPTCHA, email cooldown, or abuse-prevention.

✅ Expected Behavior:
Limit the number of magic links that can be requested per IP/email in a short time.

Introduce CAPTCHA or email-cooldown (e.g. 1 email per 30 seconds).

Return appropriate error response (429 Too Many Requests).

📷 Screenshots / Evidence:
(можно приложить скриншот входящих писем, curl-логи, HAR-файл и т.д.)

💡 Recommendations:
Implement rate-limiting (e.g. 5 requests/hour/email).

Use CAPTCHA or other bot protection mechanisms.

Add audit logging to detect such abuse.

Consider confirming client-side interaction before sending email.

