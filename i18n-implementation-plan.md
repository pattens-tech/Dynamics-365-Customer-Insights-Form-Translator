# i18n Implementation Plan - dynamics-365-form-translator

## Project Overview
Adding multi-language support to contact form with i18next library.

---

## 1. Architecture & Setup

### Library Choice
- **Library: i18next + i18next-browser-languagedetector
- **Translation Format: Embedded JavaScript object
- **Load Strategy: Inline resources (no external JSON files)

### Supported Languages
- [ ] English US (en-US) - Default
- [ ] German (de)
- [ ] Chinese Simplified (zh-Hans)

### Directory Structure
```
project-root/
├── contact-form.html (all i18n, validation, language text, and language-switching code inline)
```

---

## 2. Implementation Steps

### Phase 1: Setup
Add i18next library to HTML (via CDN). CDN: https://cdn.jsdelivr.net/npm/i18next@23.7.6/i18next.min.js  CDN: https://cdn.jsdelivr.net/npm/i18next-browser-languagedetector@7.2.0/i18nextBrowserLanguageDetector.min.js
Script Loading Order: i18next CDN → language detector CDN → i18n-config.js → DOM manipulation

Add i18next library to HTML (via CDN)
Add inline i18n initialization script in HTML `<script>` tags
Add file headers with purpose and description
Initialize translations on DOMContentLoaded event

### Phase 2: Extract & Translate Strings
Strings to translate:
- `heading` → "Contact us"
- `subheading` → "We'd love to hear from you"
- `label.firstName` → "First Name"
- `label.lastName` → "Last Name"
- `label.email` → "Email"
- `label.description` → "Description"
- `newsletter.heading` → "Newsletter"
- `newsletter.description` → "We will send you news, webinars, marketing emails. You can opt out at any time."
- `newsletter.checkbox` → "Send me the newsletter"
- `button.submit` → "Send message"
- `footer.disclaimer` → "By selecting Send message, I agree to the Terms of Service and acknowledge the Privacy policy."
- `modal.heading` → "Thank you!"
- `modal.message` → "We've received your message and will get back to you soon."
- `modal.button` → "Close"

### Phase 3: Update HTML
-dd language selector dropdown (top-right corner of form)
Replace hardcoded text with `i18next.t()` calls
Update form field attributes dynamically if needed
**CRITICAL: Preserve custom field structure** — Fields are custom built; maintain all data attributes, classes, and DOM hierarchy exactly as-is
Remove `required` attributes from all form fields (for custom validation)


### Phase 5: Add Language Switcher (Inline in HTML)
Add manual language switcher only
Persist language choice in localStorage
Initialize with English US as default
Instant re-rendering: Language changes update DOM immediately without reload
Add comments for language switching and localStorage operations
All language switcher code in `<script>` tags within HTML file

---

## 3. Code Commenting Standards

All code must include:
- **File headers** — Purpose, description, and dependencies
- **Function comments** — Function name, parameters, and what it does
- **Complex logic** — Explain non-obvious steps, especially around i18n and validation
- **Data operations** — Comment on localStorage, translation key usage, field mapping
- **Dynamics 365 specifics** — Flag any code critical to D365 form binding/submission
- **Inline comments** — For critical sections (e.g., "preserve field structure here")

Format:
```javascript
/**
 * Function description
 * @param {type} paramName - Description
 * @returns {type} Description
 */
function myFunction(paramName) {
  // Single line explanation
}
```

---

## 4. Language Selector Specifications

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

## 5. i18next Configuration

### Initialize with:
```javascript
{
  fallbackLng: 'en-US',
  lng: 'en-US',
  defaultNS: 'translation',
  ns: ['translation']
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
    },
    "errors": {
      "required": "This field is required",
      "email": "Please enter a valid email address",
      "minLength": "This field must be at least {{min}} characters"
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
    },
    "errors": {
      "required": "Dieses Feld ist erforderlich",
      "email": "Bitte geben Sie eine gültige E-Mail-Adresse ein",
      "minLength": "Dieses Feld muss mindestens {{min}} Zeichen lang sein"
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
    },
    "errors": {
      "required": "此字段为必填项",
      "email": "请输入有效的电子邮件地址",
      "minLength": "此字段必须至少包含 {{min}} 个字符"
    }
  }
}
```

---

## 6. Testing Strategy
- [ ] Test each language loads correctly
- [ ] Test language switching via selector
- [ ] Test localStorage persistence
- [ ] Test custom form validation errors display correctly
- [ ] Test error messages translate on language change
- [ ] Test Dynamics 365 form submission works correctly
- [ ] Test form accessibility with different languages

---

## 7. Deliverables
- Updated `contact-form.html` with all i18n, validation, and language-switcher code inline (within `<script>` tags)
- `locales/translations.json` (combined all languages)
- Updated `README.md` with i18n documentation and setup instructions

---

## 8. Form Submission Behavior
- Form submission to Dynamics 365 backend remains unchanged
- Language selection is stored locally but not submitted with form data
- All form field names and Dynamics 365 metadata preserved

---

## Status
Ready for Claude Code execution
