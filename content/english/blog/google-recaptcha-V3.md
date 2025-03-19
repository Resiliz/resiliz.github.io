---
title: "When verifying Recaptcha V3 Token with google Recaptcha V3 use this Content-Type in the header"
meta_title: "When verifying Recaptcha V3 Token with google Recaptcha V3 use this Content-Type in the header"
description: "When verifying Recaptcha V3 Token with google Recaptcha V3 use this Content-Type in the header"
date: 2024-08-26T05:00:00Z
image: "/images/recaptcha-v3.jpg"
categories: ["Javascript", "Recaptcha v3"]
author: "Viraj"
tags: ["javascript", "recaptcha"]
draft: false
---


I tried with application/json headers and couldn't able to verify the Recaptcha V3 token.

I found this content-type is working.

```javascript

const recaptchaFeedback = await fetch(
    "https://www.google.com/recaptcha/api/siteverify",
    {
      method: "POST",
      headers: {
        "Content-Type":
          "application/x-www-form-urlencoded", // Correct content type
      },
      body: new URLSearchParams({
        secret: RECAPTCHA_SECRET,
        response: recatatchaToken,
      }),
    }
  );







```