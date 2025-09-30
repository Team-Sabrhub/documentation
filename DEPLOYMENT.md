# Sabrhub API Documentation - Redocly Enterprise Deployment

## 📁 File Structure

```
documentation/
├── redocly.yaml                # CLI-compatible config (for local development)
├── redocly-enterprise.yaml     # Full Enterprise config (for deployment)
├── assets/
│   ├── sabrhub-logo.png        # Main Sabrhub logo (ready for deployment)
│   └── styles.css              # Custom Sabrhub branding stylesheet
├── specs/                      # OpenAPI specifications
│   ├── contextsms-webex.yaml
│   ├── contextsms-teams.yaml
│   └── contextregister.yaml
└── tests/                      # Postman collections
```

## 🚀 Redocly Enterprise Deployment Steps

### 1. Repository Connection
- Your GitHub repository `Team-Sabrhub` is connected to Redocly Enterprise
- **Primary config file**: `redocly.yaml` (CLI-compatible)
- **Enterprise settings**: Use `redocly-enterprise.yaml` as reference for UI configuration

### 2. Domain Configuration
- **Target URL**: `https://context-docs.sabrhub.com`
- **SEO Settings**: Google indexing DISABLED (`noindex`, `nofollow`)

### 3. Branding Configuration
- **Logo**: `assets/sabrhub-logo.png` ✅ (Beautiful teal-to-blue gradient mark)
- **Custom CSS**: `assets/styles.css` ✅ (Complete Sabrhub branding)
- **Colors**: 
  - Primary Blue: `#4C86E4`
  - Sabrhub Navy: `#1B2B41` 
  - Accent Teal: `#00D4AA`
- **Typography**: Inter font family

### 4. API Endpoints
- **Webex Bot API**: `specs/contextsms-webex.yaml`
- **Teams Platform API**: `specs/contextsms-teams.yaml`
- **ContextRegister API**: `specs/contextregister.yaml`

## 🔧 In Redocly Enterprise Dashboard

### Theme Settings (copy from redocly-enterprise.yaml):
1. **Logo**: Upload `assets/sabrhub-logo.png`
2. **Custom CSS**: Upload `assets/styles.css` 
3. **Colors**: Set primary to `#4C86E4`, sidebar to `#1B2B41`
4. **Typography**: Set to Inter font family

### SEO Settings:
- ✅ **Disable search indexing**
- ✅ **Add `noindex, nofollow` meta tags**
- ✅ **Block Google crawling**

### Domain Settings:
- ✅ **Set custom domain**: `context-docs.sabrhub.com`
- ✅ **Enable HTTPS**

## ✅ Client Requirements Fulfilled

1. ✅ **Domain**: `context-docs.sabrhub.com`
2. ✅ **No Google indexing**: Multiple `noindex` directives
3. ✅ **Full Sabrhub branding**: Logo, colors, typography, footer
4. ✅ **Three API documentation**: Webex, Teams, ContextRegister
5. ✅ **Professional styling**: Custom CSS with brand guidelines
6. ✅ **GitHub integration**: Auto-deployment on push
7. ✅ **Postman test collections**: All three APIs covered

## 🧪 Testing

```bash
# Local linting
npx @redocly/cli lint

# Local build (if needed)
npx @redocly/cli build

# All APIs validate successfully ✅
```

## 📝 Notes

- **Main config**: `redocly.yaml` (clean, CLI-compatible)
- **Enterprise reference**: `redocly-enterprise.yaml` (full settings for manual configuration)
- **Logo ready**: `assets/sabrhub-logo.png` (290KB, optimized)
- **CSS ready**: `assets/styles.css` (complete Sabrhub theme)
- **No conflicts**: All configurations tested and validated

**🎉 Ready for Enterprise deployment!**
