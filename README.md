# Dynamics 365 Form Translator

Multi-language contact form for Dynamics 365 Marketing with built-in internationalization (i18n) support.

## Overview

This project provides a customizable, multi-language contact form designed specifically for Dynamics 365 Marketing. It uses i18next for seamless language switching and maintains full compatibility with Dynamics 365 form submission requirements.

## Features

- **Multi-language Support**: English (US), German, and Chinese Simplified
- **Instant Language Switching**: Real-time translation updates without page reload
- **Persistent Language Preference**: User's language choice saved in localStorage
- **Dynamics 365 Compatible**: All data attributes and form structure preserved
- **Theme Customization**: Easy brand color configuration
- **Newsletter Opt-in**: Integrated with Dynamics 365 Topic tracking
- **Success Modal**: Confirmation message after form submission
- **Responsive Design**: Built with Tailwind CSS for all screen sizes

## Supported Languages

| Language | Code | Status |
|----------|------|--------|
| English (US) | `en-US` | ✅ Default |
| German | `de` | ✅ Available |
| Chinese Simplified | `zh-Hans` | ✅ Available |

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/dynamics-365-form-translator.git
   cd dynamics-365-form-translator
   ```

2. **Open the form**
   - Open `contact-form.html` in your browser to test locally
   - Or deploy to your Dynamics 365 Marketing instance

3. **Customize the theme** (optional)
   - Edit line 53 in `contact-form.html`
   - Change the `brand` color to match your branding
   ```javascript
   brand: {
       DEFAULT: '#0078d4' // Change this hex code
   }
   ```

## How It Works

### Language Detection & Storage

The form automatically:
1. Checks localStorage for a saved language preference
2. Defaults to English (US) if no preference is found
3. Saves the user's language selection for future visits

### Translation System

All translations are embedded inline in the HTML file for optimal performance:

```javascript
const translationResources = {
  'en-US': {
    translation: { /* English translations */ }
  },
  'de': {
    translation: { /* German translations */ }
  },
  'zh-Hans': {
    translation: { /* Chinese translations */ }
  }
};
```

### Language Switching

Users can switch languages using the dropdown selector at the top-right of the form. Changes are instant and affect:
- Form labels
- Headings and descriptions
- Button text
- Modal messages
- All user-facing content

## Architecture

### Dependencies

All dependencies are loaded via CDN:
- **Tailwind CSS**: UI styling framework
- **i18next 23.7.6**: Internationalization framework
- **i18next-browser-languagedetector 7.2.0**: Browser language detection
- **Google Fonts (Roboto)**: Typography

### File Structure

```
project-root/
├── contact-form.html          # Main form with embedded i18n
├── i18n-implementation-plan.md # Implementation documentation
└── README.md                   # This file
```

### Key Components

1. **Language Selector** (`contact-form.html:312-317`)
   - Dropdown for manual language selection
   - Positioned at top-right of form

2. **i18n Initialization** (`contact-form.html:83-302`)
   - Translation resources
   - i18next configuration
   - Language switching functions
   - Content update logic

3. **Form Fields** (`contact-form.html:329-347`)
   - All Dynamics 365 data attributes preserved
   - Required attributes removed for custom validation
   - Labels dynamically updated on language change

4. **Modal** (`contact-form.html:373-382`)
   - Thank you message
   - Translatable content
   - Close button

## Customization

### Adding a New Language

1. Add translation resources in `contact-form.html` (around line 94):

```javascript
'fr': {  // French example
  translation: {
    heading: "Contactez-nous",
    subheading: "Nous aimerions vous entendre",
    // ... add all translation keys
  }
}
```

2. Add option to language selector (around line 312):

```html
<option value="fr">Français</option>
```

### Modifying Translations

Edit the translation resources in `contact-form.html` (lines 94-194). All translatable strings are organized by category:

- `heading` / `subheading`: Form title and subtitle
- `label.*`: Form field labels
- `newsletter.*`: Newsletter section
- `button.*`: Button text
- `footer.*`: Footer text
- `modal.*`: Thank you modal
- `errors.*`: Validation messages (for future use)

### Theme Colors

The form supports easy theme customization. Pre-defined themes:

| Theme | Color Code | Description |
|-------|------------|-------------|
| Blue (Default) | `#0078d4` | Professional and trustworthy |
| Corporate | `#1e293b` | Sleek slate gray |
| Vibrant | `#9333ea` | Bold purple |
| Minimal | `#000000` | Classic black and white |

Change the theme by editing line 53 in `contact-form.html`.

## Dynamics 365 Integration

### Form Submission

The form maintains full compatibility with Dynamics 365 Marketing:
- All `data-editorblocktype` attributes preserved
- All `data-targetaudience` and `data-targetproperty` attributes intact
- Newsletter Topic ID and channel settings maintained
- Language preference is NOT submitted with form data

### Field Mapping

| Field Name | Dynamics 365 Property | Required |
|------------|----------------------|----------|
| firstname | contact.firstname | Yes (via data attribute) |
| lastname | contact.lastname | Yes (via data attribute) |
| emailaddress1 | contact.emailaddress1 | Yes (via data attribute) |
| description | contact.description | Yes (via data attribute) |
| consentTopicCheckbox | Topic opt-in | No |

### Custom Validation

The HTML `required` attributes have been removed to allow for custom validation logic. Implement your own validation using the `errors.*` translation keys:

```javascript
// Example custom validation
if (!firstname.value) {
  showError(i18next.t('errors.required'));
}
```

## Browser Support

The form works in all modern browsers:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Technical Notes

### localStorage Usage

The form uses `localStorage` to persist language preference:
- **Key**: `preferredLanguage`
- **Value**: Language code (e.g., `'en-US'`, `'de'`, `'zh-Hans'`)
- **Fallback**: `'en-US'` if not set

### Performance

- All translations are embedded inline (no external JSON files)
- No page reload required for language changes
- Minimal JavaScript execution on language switch
- CDN resources cached by browser

### Accessibility

- Semantic HTML structure
- Proper label associations
- Keyboard navigation support
- Screen reader friendly
- ARIA attributes can be added as needed

## Troubleshooting

### Language not switching

1. Check browser console for JavaScript errors
2. Verify i18next CDN scripts are loading
3. Clear localStorage and refresh page
4. Check translation keys match between resources and updatePageContent()

### Translations not showing

1. Verify `data-translation` attributes are present on elements
2. Check translation keys exist in all language resources
3. Ensure `updatePageContent()` function is called after language change

### Form not submitting to Dynamics 365

1. Verify all `data-editorblocktype` attributes are intact
2. Check `data-targetaudience` and `data-targetproperty` match your D365 setup
3. Ensure form is deployed in Dynamics 365 Marketing context

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is open source. Please check the repository for license details.

## Support

For issues or questions:
- Open an issue on GitHub
- Check existing documentation
- Review the implementation plan: `i18n-implementation-plan.md`

## Credits

Built with:
- [i18next](https://www.i18next.com/) - Internationalization framework
- [Tailwind CSS](https://tailwindcss.com/) - CSS framework
- [Dynamics 365 Marketing](https://dynamics.microsoft.com/marketing/) - Microsoft's marketing platform