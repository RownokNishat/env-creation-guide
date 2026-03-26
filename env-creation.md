## Step-by-Step: Getting All Credentials

---

### 1. Gmail SMTP (Email)

1. Go to [myaccount.google.com](https://myaccount.google.com)
2. **Security** → Enable **2-Step Verification** (required)
3. Search "App passwords" in the search bar → click it
4. Select app: **Mail**, device: **Other** → type "Healthcare App" → click **Generate**
5. Copy the 16-character password shown

```
EMAIL_SENDER_SMTP_USER=yourgmail@gmail.com
EMAIL_SENDER_SMTP_PASS=xxxx xxxx xxxx xxxx   ← the 16-char password
EMAIL_SENDER_SMTP_FROM=yourgmail@gmail.com
```

---

### 2. Google OAuth (Google Login)

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. **Create a new project** → name it "Healthcare App"
3. Left menu → **APIs & Services** → **OAuth consent screen**
   - User Type: **External** → click Create
   - Fill app name, support email → Save
4. Left menu → **Credentials** → **+ Create Credentials** → **OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Under **Authorized redirect URIs** → Add: `https://your-app.vercel.app/api/auth/callback/google`
   - Click **Create**
5. Copy the **Client ID** and **Client Secret**

```
GOOGLE_CLIENT_ID=xxxxx.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=GOCSPX-xxxxxxx
```

---

### 3. Cloudinary (File Uploads)

1. Go to [cloudinary.com](https://cloudinary.com) → Sign up / Log in
2. You land on the **Dashboard** automatically
3. Copy the 3 values visible right there:

```
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=123456789012345
CLOUDINARY_API_SECRET=xxxxxxxxxxxxxxxxxxx
```

---

### 4. Stripe (Payments)

**Secret Key:**
1. Go to [dashboard.stripe.com](https://dashboard.stripe.com)
2. Sign up / Log in → top right toggle to **Test mode** (for now)
3. Left menu → **Developers** → **API keys**
4. Copy **Secret key** (`sk_test_...`)

**Webhook Secret:**
1. Left menu → **Developers** → **Webhooks**
2. Click **+ Add endpoint**
3. Endpoint URL: `https://your-app.vercel.app/webhook`
4. Click **Select events** → choose `payment_intent.succeeded`, `checkout.session.completed` → Add
5. Click the webhook → copy **Signing secret** (`whsec_...`)

```
STRIPE_SECRET_KEY=sk_test_xxxxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxx
```

---

### 5. Super Admin (Your Choice)

Just pick any email + strong password. This is only used to seed the first admin account into your DB.

```
SUPER_ADMIN_EMAIL=admin@yourdomain.com
SUPER_ADMIN_PASSWORD=StrongPassword123!
```

---

### 6. Database URL (if not done yet)

1. Go to [neon.tech](https://neon.tech) → Sign up → **New Project**
2. After creation, go to **Dashboard** → **Connection string**
3. Select **Prisma** from the dropdown → copy the URL

```
DATABASE_URL=postgresql://user:pass@host/db?sslmode=require
```

After adding all variables to Vercel, run migrations once:
```bash
$env:DATABASE_URL="your-production-url"; npx prisma migrate deploy
```