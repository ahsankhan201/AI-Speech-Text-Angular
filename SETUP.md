# Quick Setup Guide

This guide will help you get the AI Speech Angular application up and running in just a few minutes.

## Prerequisites Check

Before starting, ensure you have:
- ✅ Node.js v20+ installed (`node --version`)
- ✅ npm v10+ installed (`npm --version`)
- ✅ OpenAI API account with credits

## Setup Steps

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Environment

Create environment configuration files:

```bash
# Copy the template for development
cp src/environments/environment.template.ts src/environments/environment.development.ts

# Copy the template for production
cp src/environments/environment.template.ts src/environments/environment.ts
```

### 3. Add Your API Key

Open `src/environments/environment.development.ts` and add your OpenAI API key:

```typescript
export const environment = {
  production: false,
  openai: {
    apiKey: 'sk-your-actual-api-key-here', // Replace with your key
    // ... rest of config
  }
};
```

**Get your API key:**
1. Go to [OpenAI Platform](https://platform.openai.com/api-keys)
2. Sign in or create an account
3. Click "Create new secret key"
4. Copy the key immediately (you won't see it again)

### 4. Start Development Server

```bash
npm start
```

Open your browser to [http://localhost:4200](http://localhost:4200)

## Verification

1. Navigate to "Speech to Text" in the sidebar
2. Click the microphone button
3. Grant microphone permissions when prompted
4. Speak something
5. Click stop
6. You should see:
   - Your transcribed speech
   - An AI response

## Troubleshooting

### "Invalid API key" error
- Double-check your API key in the environment file
- Ensure there are no extra spaces or quotes
- Verify the key is active on OpenAI platform

### "Could not access microphone"
- Grant microphone permissions in browser settings
- Ensure you're using HTTPS (or localhost)
- Check if another app is using the microphone

### No response from AI
- Check your OpenAI account has credits
- Verify internet connection
- Check browser console for detailed errors

### Build errors
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

## Next Steps

- ✨ Customize the system prompt in `src/app/constants/app.constants.ts`
- 🎨 Adjust styles in component `.scss` files
- ⚙️ Configure model parameters in environment files
- 📖 Read the full [README.md](./README.md) for architecture details

## Important Security Notes

⚠️ **Never commit your API keys to git**

The environment files are already gitignored, but always verify:
```bash
git status
# Should NOT show environment.ts files
```

## Support

Having issues? Check:
1. Node.js and npm versions
2. OpenAI API key validity
3. Browser console for errors
4. Network tab for API call failures

For more help, create an issue with:
- Error messages
- Browser console logs
- Steps to reproduce

