# i18n Implementation Plan - dynamics-365-form-translator

## Project Overview
Adding multi-language support to contact form with i18next library.

---

## 1. Architecture & Setup

### Library Choice
- **Library**: i18next + i18next-browser-languagedetector
- **Translation Format**: JSON
- **Load Strategy**: Static JSON files (no build required)

### Supported Languages
- [ ] English US (en-US) - Default
- [ ] German (de)
- [ ] Chinese Simplified (zh-Hans)

### Directory Structure
```
project-root/
├── contact-form.html (updated)
├── locales/
│   └── translations.json (combined all languages)
└── js/
    └── i18n-config.js
```

---

## 2. Implementation Steps

### Phase 1: Setup
- [ ] Create `/locales` directory
- [ ] Create combined `translations.json` file
- [ ] Add i18next library to HTML (via CDN)
- [ ] Create i18n config file (i18n-config.js)

### Phase 2: Extract & Translate Strings
Strings to translate:
- `heading` → "Contact us"
- `subheading` → "We'd love to hear from you"
- `label.firstName` → "First Name"
- `label.lastName` → "Last Name"
- `label.email` → "Email"
- `label.description` → "Description"
- `placeholder.firstName` → "First Name"
- `placeholder.lastName` → "Last Name"
- `placeholder.email` → "Email"
- `placeholder.description` → "Description"
- `newsletter.heading` → "Newsletter"
- `newsletter.description` → "We will send you news, webinars, marketing emails. You can opt out at any time."
- `newsletter.checkbox` → "Send me the newsletter"
- `button.submit` → "Send message"
- `footer.disclaimer` → "By selecting Send message, I agree to the Terms of Service and acknowledge the Privacy policy."
- `modal.heading` → "Thank you!"
- `modal.message` → "We've received your message and will get back to you soon."
- `modal.button` → "Close"

### Phase 3: Update HTML
- [ ] Add language selector dropdown (top-right corner of form)
- [ ] Replace hardcoded text with `i18next.t()` calls
- [ ] Update form field attributes dynamically if needed
- [ ] Preserve all Dynamics 365 metadata attributes

### Phase 4: Add Language Detection & Switching
- [ ] Auto-detect browser language
- [ ] Fallback to English US if language not supported
- [ ] Add manual language switcher
- [ ] Persist language choice in localStorage

---

## 3. Language Selector Specifications

### Position
- Top-right corner of form container
- Inside the main form div, above heading

### Style Options
- Dropdown select element (preferred for simplicity)
- Styling with Tailwind classes to match form design

### Functionality
- Immediate language switching on selection
- Display current language
- Load translation strings dynamically

---

## 4. i18next Configuration

### Initialize with:
```javascript
{
  fallbackLng: 'en-US',
  defaultNS: 'translation',
  detection: {
    order: ['localStorage', 'navigator'],
    caches: ['localStorage']
  }
}
```

### Combined Translation JSON Structure
```json
{
  "en-US": {
    "heading": "Contact us",
    "subheading": "We'd love to hear from you",
    "label": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "email": "Email",
      "description": "Description"
    },
    "placeholder": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "email": "Email",
      "description": "Description"
    },
    "newsletter": {
      "heading": "Newsletter",
      "description": "We will send you news, webinars, marketing emails. You can opt out at any time.",
      "checkbox": "Send me the newsletter"
    },
    "button": {
      "submit": "Send message"
    },
    "footer": {
      "disclaimer": "By selecting Send message, I agree to the Terms of Service and acknowledge the Privacy policy."
    },
    "modal": {
      "heading": "Thank you!",
      "message": "We've received your message and will get back to you soon.",
      "button": "Close"
    }
  },
  "de": {
    "heading": "Kontaktieren Sie uns",
    "subheading": "Wir würden gerne von Ihnen hören",
    "label": {
      "firstName": "Vorname",
      "lastName": "Nachname",
      "email": "E-Mail",
      "description": "Beschreibung"
    },
    "placeholder": {
      "firstName": "Vorname",
      "lastName": "Nachname",
      "email": "E-Mail",
      "description": "Beschreibung"
    },
    "newsletter": {
      "heading": "Newsletter",
      "description": "Wir senden Ihnen Nachrichten, Webinare und Marketing-E-Mails. Sie können sich jederzeit abmelden.",
      "checkbox": "Senden Sie mir den Newsletter"
    },
    "button": {
      "submit": "Nachricht senden"
    },
    "footer": {
      "disclaimer": "Mit der Auswahl 'Nachricht senden' stimme ich den Nutzungsbedingungen zu und erkenne die Datenschutzrichtlinie an."
    },
    "modal": {
      "heading": "Vielen Dank!",
      "message": "Wir haben Ihre Nachricht erhalten und werden uns bald bei Ihnen melden.",
      "button": "Schließen"
    }
  },
  "zh-Hans": {
    "heading": "联系我们",
    "subheading": "我们很乐意听到您的声音",
    "label": {
      "firstName": "名字",
      "lastName": "姓氏",
      "email": "电子邮件",
      "description": "描述"
    },
    "placeholder": {
      "firstName": "名字",
      "lastName": "姓氏",
      "email": "电子邮件",
      "description": "描述"
    },
    "newsletter": {
      "heading": "新闻通讯",
      "description": "我们将向您发送新闻、网络研讨会和营销电子邮件。您可以随时选择退出。",
      "checkbox": "给我发送新闻通讯"
    },
    "button": {
      "submit": "发送消息"
    },
    "footer": {
      "disclaimer": "通过选择'发送消息'，我同意服务条款并承认隐私政策。"
    },
    "modal": {
      "heading": "谢谢！",
      "message": "我们已收到您的消息，将很快与您联系。",
      "button": "关闭"
    }
  }
}
```

---

## 5. Testing Strategy
- [ ] Test each language loads correctly
- [ ] Test language switching via selector
- [ ] Test localStorage persistence
- [ ] Test browser language detection fallback
- [ ] Test Dynamics 365 form submission works correctly
- [ ] Test form accessibility with different languages

---

## 6. Deliverables
- Updated `contact-form.html` with i18n integration
- `js/i18n-config.js` configuration file
- `locales/translations.json` (combined all languages)
- Language selector UI component
- Updated `README.md` with i18n documentation and setup instructions

---

## 7. Form Submission Behavior
- Form submission to Dynamics 365 backend remains unchanged
- Language selection is stored locally but not submitted with form data
- All form field names and Dynamics 365 metadata preserved

---

## Status
Ready for Claude Code execution
