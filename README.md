# Abusing-Contact-Form-Submission-via-Burp-Suite-Sniper-Attack-Rate-Limit-Bypass-
This report highlights a vulnerability in a public contact form lacking server-side rate-limiting. Despite input validation, Burp Suite’s Sniper attack can repeatedly send payloads from the same IP, risking abuse like XSS bypass, spam, or backend resource overload. 
![Blue Purple Futuristic Virus Hacks Youtube Thumbnail (1)](https://github.com/user-attachments/assets/6eb71b56-bd9d-42e3-a51d-2cc91ed23e82)
# Objective
1. Assess the security of a web contact form.
2. Identify server-side misconfigurations such as the lack of rate-limiting.
2. Demonstrate how Burp Suite’s Intruder tool (Sniper mode) can be used to automate abuse attempts.
3. Emphasize that input validation alone is insufficient without proper backend controls.
   
# Form Description
1. Name (text input)
2. Contact Number (text input)
3. Message (textarea)
4. Submit button
While input filtering was present (blocking standard script tags), the server accepted unlimited form submissions from the same IP without any delay or restrictions.

# Attack Methodology Using Burp Suite

# Step 1: Proxy Setup
- Configure Burp Suite to intercept traffic from the browser.
- Ensure the proxy is set to 127.0.0.1:8080
# Step 2: Capture a Valid Form Submission
- Fill in valid data in all form fields and click submit.
- Intercept the HTTP POST request using Burp Suite.
# Step 3: Send Request to Intruder
- Right-click the intercepted request and choose Send to Intruder.
- Go to the Intruder tab and select Sniper as the attack type.
# Step 4: Define Payload Position
- Choose the input parameter to test (e.g., name, message).
- Highlight the parameter value and wrap it with § to mark it for payload injection:
```ruby
name=§John Doe§
```

# Step 5: Insert Payload
- In the Payloads tab, provide a list of test inputs, including obfuscated or encoded XSS attempts such as:
```ruby
<scr<script>ipt>alert(1)</script>
<img src=x onerror=alert('XSS')>
"><svg/onload=confirm(1)>
```

# Step 6: Launch the Attack
- Start the attack.
- Burp Suite will iterate through each payload and send it to the server.
- Monitor responses to check for payload reflections, input handling, and response codes.

  # Findings
- The application accepts an unlimited number of form submissions from a single IP address.
- Payloads were filtered at a basic level, but the lack of submission controls enabled brute-force-style injection attempts.
- This can lead to:
- XSS bypass discovery
- Admin inbox spam/flooding
- Log injection or poisoning
- Form abuse automation
  
# Recommendations
1. Implement Rate Limiting: Restrict form submissions per IP (e.g., max 5 per minute).
2. Use CAPTCHA: Add challenge-response mechanisms to prevent automated submissions.
3. Enable Server-Side Input Validation and Encoding: Go beyond client-side filters.
4. Monitor for Abuse Patterns: Implement logging and alerting for unusual form activity.
5. Harden Response Handling: Ensure user input is never directly reflected without context-aware encoding.
   
# Conclusion
This case illustrates how input validation alone does not guarantee application security. Server-side protections such as rate limiting, CAPTCHA, and input encoding must work in tandem to prevent automated attacks and abuse scenarios.



