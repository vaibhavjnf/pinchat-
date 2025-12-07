# Privacy Policy for PinChat

**Effective Date**: December 7, 2025  
**Last Updated**: December 7, 2025

## Overview
PinChat ("we," "our," "us") converts Pinterest pins and uploaded images into AI-generated prompts. This Privacy Policy explains how we collect, use, and protect your information.

## Information We Collect

### Information You Provide
- **Images**: Photos or Pinterest pin URLs you upload for analysis
- **Account Data**: Email and password if you create an account (optional)
- **Generated Prompts**: Prompts we create and you choose to save

### Automatically Collected
- **Usage Data**: Pages visited, features used, session duration
- **Device Information**: Browser type, IP address, device identifiers
- **Cookies**: Session tokens and preferences (see Cookie Policy below)

## How We Use Your Information

- **Prompt Generation**: Process images through Google Gemini AI to analyze style and generate prompts
- **Service Improvement**: Analyze usage patterns to enhance features
- **Account Management**: Maintain your saved prompt history (if logged in)
- **Communication**: Send service updates and respond to support requests

## Data Storage and Processing

- **Images**: Temporarily processed through Gemini API, not permanently stored unless you save them
- **Prompts**: Stored in Supabase database if you save them to history
- **Third-Party Processing**: Google Gemini analyzes images; Pinterest metadata may be scraped from public URLs
- **Data Retention**: Saved prompts stored indefinitely unless you delete them; session data expires after 30 days

## Data Sharing

We DO NOT sell your personal information. We share data only with:

- **Google Gemini**: Images sent for AI analysis (subject to Google's privacy policy)
- **Supabase**: Database hosting for accounts and saved prompts
- **Vercel**: Application hosting and analytics
- **Pinterest**: Public pin metadata accessed via URL (if you provide pin links)

## Your Rights

You have the right to:
- **Access**: Request a copy of your data
- **Delete**: Remove your account and all saved prompts
- **Opt-Out**: Use the service without creating an account (localStorage only)
- **Data Portability**: Export your saved prompts as JSON

To exercise these rights, contact us at [your-email@pinchat.com].

## Security

We implement industry-standard security measures:
- HTTPS encryption for data transmission
- Secure authentication via Supabase Auth
- API keys stored as environment variables (never client-side)
- Regular security audits

## Cookies and Tracking

We use:
- **Essential Cookies**: Session management and authentication
- **Analytics Cookies**: Vercel Analytics for usage statistics (anonymized)
- **Local Storage**: Save preferences and history without an account

You can disable cookies in your browser, but this may limit functionality.

## Children's Privacy

PinChat is not intended for users under 13. We do not knowingly collect data from children.

## Third-Party Links

Our service may link to Pinterest and other external sites. We are not responsible for their privacy practices.

## Changes to This Policy

We may update this policy periodically. Continued use after changes constitutes acceptance. Major changes will be notified via email (for registered users).

## Data Processing for AI

**Important**: Images you upload are sent to Google Gemini for analysis. Google may process this data according to their AI services terms. We recommend not uploading sensitive or private images.

## International Users

Data is processed in the United States. By using PinChat, you consent to data transfer to the US.

## Contact Us

For privacy questions or data requests:
- **Email**: [your-email@pinchat.com]
- **Address**: [Your Business Address]

---

## Cookie Policy

### Essential Cookies
- `session`: Authentication token (7 days)
- `pinchat_prefs`: User preferences (1 year)

### Analytics Cookies
- `vercel_analytics`: Anonymized usage tracking (30 days)

### Managing Cookies
Most browsers allow cookie management via settings. Disabling essential cookies will prevent login functionality.

---

**By using PinChat, you agree to this Privacy Policy.**