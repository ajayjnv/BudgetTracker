# Budget Tracker Web App

A simple personal budget tracker built with Node.js, Express, EJS and MongoDB. The app supports user registration/sign-in, JWT-based session cookies, 
creating budgets (weekly/monthly/yearly), tracking spendings (called "buddies"), a basic savings flow and profile management.

- Runtime: Node.js (Express)
- View engine: EJS
- Database: MongoDB (Mongoose)
- Auth: JWT stored in an HttpOnly cookie
- Password hashing: bcrypt

## Features

- Register / Login / Logout
- Forgot password (security question)
- Create budgets (week / month / year)
- Add spending entries (category: primary / secondary / others)
- Track savings and transfer to current/new budget
- Upload profile image (via a form)
- Basic filtering (by year/month)
- Static assets served from `public/`

## Quick start

1. Clone or copy project into `D:\project` (already your working folder).
2. Install dependencies:

   ```powershell
   cd /d D:\project
   npm install
   ```

3. Create a `.env` file in the project root with at minimum:

   ```env
   MONGO_URI=mongodb+srv://<user>:<password>@cluster0.mongodb.net/mydb?retryWrites=true&w=majority
   SECRET_KEY=your_jwt_secret
   NODE_ENV=development
   PORT=3000
   ```

4. Start the app:

   ```powershell
   # development
   node index.js
   # or, if package.json has a start script:
   npm run dev
   ```

5. Open http://localhost:3000

## Project layout

- `index.js` — main Express app and all route handlers
- `views/` — EJS templates (pages and partials)
- `public/` — static assets (CSS and images)
- `package.json` — (not shown here) dependencies & scripts

## API / Routes (overview)

- GET `/` — start page
- GET `/register` — register page (rendered)
- GET `/login` — login page (rendered)
- GET `/forgotpassword` — forgot password page
- GET `/changepassword` — change password (protected)
- GET `/changeprofile` — change profile (protected)
- GET `/home` — dashboard (protected)
- GET `/present` — active budget view (protected)
- GET `/past` — past budgets view (protected)
- POST `/adduser` — register a new user
- POST `/signin` — sign in and create JWT cookie
- POST `/showpassword` — reveal stored repassword via security question (existing behavior)
- POST `/submitbudget` — create a new budget for the signed-in user
- POST `/savingsadd` — transfer savings into current/new budget
- POST `/addtracks` — add a spending (buddy) entry to the active budget
- POST `/check` — fetch buddy entries for a budget
- POST `/search` — search budgets by year/month for the signed-in user
- POST `/fetchdata` — returns JSON of tracker records for charting/filtering
- POST `/changepassword` — change password for signed-in user
- POST `/upload` — update profile image for signed-in user
- GET `/logout` — clear JWT cookie and logout

Protected routes are guarded by `authToken` middleware which verifies an HttpOnly cookie `auth_token`.

## Data models (Mongoose schemas)

- user
  - full_name, email, gender, password (hashed), repassword (stored as provided), security_question, security_answer, profile_img, savingsAmount
- tracker
  - email, budget_id, budget, budget_type, startDate, endDate, totalspending, totalremaining, primary, secondary, others, status
- buddie
  - email, budget_id, amount, category, purpose, paymentmode, paymentreceiver, date
