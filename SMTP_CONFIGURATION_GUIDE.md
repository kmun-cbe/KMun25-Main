# SMTP Configuration Guide for K-MUN 2025 Mailer

## Environment File Configuration

Create a `.env` file in the `backend/` directory with the following content:

### Complete .env File Template

```env
# Database Configuration
DATABASE_URL="postgresql://username:password@localhost:5432/database_name"

# JWT Configuration
JWT_SECRET="your-jwt-secret-key-here"

# Payment Gateway Configuration (Razorpay)
RAZORPAY_KEY_ID="your-razorpay-key-id"
RAZORPAY_KEY_SECRET="your-razorpay-key-secret"

# Gmail SMTP Configuration
GMAIL_SMTP_HOST="smtp.gmail.com"
GMAIL_SMTP_PORT=587
GMAIL_SMTP_USER="your-gmail@gmail.com"
GMAIL_SMTP_PASS="your-gmail-app-password"

# Outlook/Hotmail SMTP Configuration
OUTLOOK_SMTP_HOST="smtp-mail.outlook.com"
OUTLOOK_SMTP_PORT=587
OUTLOOK_SMTP_USER="your-email@outlook.com"
OUTLOOK_SMTP_PASS="your-outlook-password"

# Server Configuration
NODE_ENV="development"
PORT=3001
```

## Gmail SMTP Setup

### 1. Enable 2-Factor Authentication
- Go to your Google Account settings
- Navigate to Security → 2-Step Verification
- Enable 2-Step Verification if not already enabled

### 2. Generate App Password
- Go to Google Account → Security → App passwords
- Select "Mail" as the app
- Select "Other" as the device and enter "K-MUN Mailer"
- Copy the generated 16-character password

### 3. Gmail Configuration
```env
GMAIL_SMTP_HOST="smtp.gmail.com"
GMAIL_SMTP_PORT=587
GMAIL_SMTP_USER="your-gmail@gmail.com"
GMAIL_SMTP_PASS="your-16-character-app-password"
```

## Outlook/Hotmail SMTP Setup

### 1. Enable Less Secure Apps (if needed)
- Go to Microsoft Account security settings
- Enable "Less secure app access" or use App Passwords

### 2. Outlook Configuration
```env
OUTLOOK_SMTP_HOST="smtp-mail.outlook.com"
OUTLOOK_SMTP_PORT=587
OUTLOOK_SMTP_USER="your-email@outlook.com"
OUTLOOK_SMTP_PASS="your-outlook-password"
```

## Alternative Outlook Configuration (Office 365)

If you're using Office 365 or business Outlook:

```env
OUTLOOK_SMTP_HOST="smtp.office365.com"
OUTLOOK_SMTP_PORT=587
OUTLOOK_SMTP_USER="your-email@yourdomain.com"
OUTLOOK_SMTP_PASS="your-office365-password"
```

## How the Mailer Works

The mailer system now supports both Gmail and Outlook providers:

1. **User Selection**: When composing an email, users can choose between Gmail or Outlook
2. **Dynamic SMTP**: The system automatically uses the appropriate SMTP configuration based on the selected provider
3. **Fallback**: If no provider is specified, it defaults to Gmail

## Testing the Configuration

### Test Gmail Configuration
```bash
curl -X POST http://localhost:3001/api/mailer/test \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "email": "test@example.com",
    "subject": "Test Email",
    "message": "This is a test email from K-MUN 2025"
  }'
```

### Test Outlook Configuration
The system will automatically use Outlook SMTP when the user selects "Outlook" in the mailer interface.

## Security Notes

1. **Never commit .env files** to version control
2. **Use App Passwords** instead of regular passwords for Gmail
3. **Enable 2FA** on all email accounts used for SMTP
4. **Rotate passwords** regularly
5. **Use environment-specific configurations** for development, staging, and production

## Troubleshooting

### Gmail Issues
- Ensure 2FA is enabled
- Use App Password, not regular password
- Check if "Less secure app access" is disabled (it should be)

### Outlook Issues
- Verify the SMTP server address
- Check if the account has SMTP access enabled
- Try using App Passwords if available

### General Issues
- Check firewall settings
- Verify port 587 is not blocked
- Ensure the email account exists and is active

## Production Deployment

For production deployment, set these environment variables in your hosting platform:

```env
GMAIL_SMTP_USER="production-gmail@yourdomain.com"
GMAIL_SMTP_PASS="production-app-password"
OUTLOOK_SMTP_USER="production-outlook@yourdomain.com"
OUTLOOK_SMTP_PASS="production-password"
```

## Support

If you encounter issues:
1. Check the server logs for detailed error messages
2. Verify your SMTP credentials
3. Test with a simple email client first
4. Contact your hosting provider if ports are blocked
