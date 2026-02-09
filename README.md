# WorkTime Logging Application

A comprehensive Flask-based time tracking application designed to log work hours, monitor daily productivity, and analyze time trends through interactive charts. Track your login times, monitor deviation from target hours, and visualize your work patterns with detailed analytics.

## Features

- **User Authentication**
  - Secure user registration and login
  - Session-based authentication
  - Password hashing using Werkzeug
  - Account isolation (each user sees only their data)

- **Time Entry Tracking**
  - Log daily in-time and out-time
  - Automatic calculation of total hours worked
  - View weekly time entries in organized tables
  - Edit or delete existing time entries
  - Deviation tracking from 8:42 hours (target work hours)

- **Weekly & Monthly Navigation**
  - Navigate between different weeks with ◀ and ▶ buttons
  - Navigate between different months
  - Return to current week/month with "This Week" / "This Month" button
  - Easy viewing of historical time entries

- **Analytics & Visualization**
  - Daily average hours chart (This Week)
  - Weekly average hours chart (This Month)
  - Monthly average hours chart (This Year)
  - Interactive Chart.js visualizations with hover tooltips
  - Real-time statistics showing weekly and monthly totals

- **Deviation Tracking**
  - Compare actual hours against 8:42 target hours per day
  - View deviation in minutes (positive/negative)
  - Weekly and monthly deviation summaries
  - Color-coded indicators for visual clarity

- **Theme Support**
  - Light and Dark mode toggle
  - Customizable accent colors for both themes
  - Persistent theme preference in localStorage
  - Responsive design that works on all devices

- **Security**
  - Password hashing (not stored in plain text)
  - Session-based authentication
  - SQL injection protection (parameterized queries)
  - Input validation
  - User data isolation

## Technology Stack

- **Backend**: Flask (Python)
- **Database**: SQLite
- **Frontend**: HTML5 with CSS3 (responsive design)
- **Charts**: Chart.js (interactive visualizations)
- **Security**: Werkzeug for password hashing
- **Styling**: Custom CSS with theme support (Light/Dark mode)

## Project Structure

```
worktime-application/
├── app.py                          # Main Flask application
├── requirements.txt                # Python dependencies
├── users.db                        # SQLite database (created on first run)
├── README.md                       # Project documentation
└── templates/
    ├── base.html                   # Base template with header
    ├── login.html                  # Login page
    ├── signup.html                 # Registration page
    ├── welcome.html                # Welcome page
    ├── profile.html                # User profile page
    ├── notes.html                  # Notes listing
    ├── tracker.html                # Main time tracking dashboard
    ├── edit_note.html              # Note editing page
    └── admin_*.html                # Admin dashboard pages
```

## Installation & Setup

### 1. Clone or Download the Project

```bash
cd "c:\Users\RRADHAKR\OneDrive - Volvo Cars\Beam Shape\RamjithR\My Projects\worktime-application"
```

### 2. Create a Virtual Environment (Optional but Recommended)

```bash
python -m venv venv
venv\Scripts\activate  # On Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Application

```bash
python app.py
```

The application will start on `http://localhost:5001`

## Usage

### First Time Setup
1. Navigate to `http://localhost:5001`
2. Sign up with a new account or login with existing credentials
3. You'll be redirected to the Time Tracker dashboard

### Logging Time Entries
1. Go to **Time Tracker** page
2. In the "Add Today's Time Entry" form:
   - Select the date (defaults to today)
   - Enter "In Time" (e.g., 09:00)
   - Enter "Out Time" (e.g., 18:00)
   - Click "Add Entry"
3. The entry is calculated and added to the weekly table

### Viewing Weekly Time Entries
1. Time entries are displayed in a table showing:
   - Date and day of the week
   - In time and out time
   - Total hours worked
   - Deviation from target (8:42 hours)
2. Use navigation buttons to view previous/next weeks:
   - ◀ button: Previous week
   - "This Week" button: Return to current week
   - ▶ button: Next week

### Viewing Analytics
1. **Daily Chart (This Week)**: Shows average hours for each day
2. **Weekly Chart (This Month)**: Shows average hours for each week
3. **Monthly Chart (This Year)**: Shows average hours for each month
4. Hover over bars to see exact values

### Managing Your Account
- **Profile**: View your account information
- **Settings**: Customize theme (Light/Dark) and accent colors
- **Logout**: Sign out of your account

## Database Schema

### Users Table
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    email TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,           -- Hashed password
    full_name TEXT NOT NULL,
    is_admin INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### Time Entries Table
```sql
CREATE TABLE time_entries (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    date TEXT NOT NULL,               -- Date of entry (YYYY-MM-DD format)
    in_time TEXT NOT NULL,            -- Clock in time (HH:MM format)
    out_time TEXT NOT NULL,           -- Clock out time (HH:MM format)
    total_hours REAL NOT NULL,        -- Calculated total hours worked
    deviation_minutes INTEGER NOT NULL, -- Deviation from 8:42 target hours
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, date),
    FOREIGN KEY (user_id) REFERENCES users(id)
)
```

### Login History Table
```sql
CREATE TABLE login_history (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    username TEXT NOT NULL,
    login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ip_address TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
)
```

## Security Features

1. **Password Hashing**
   - Uses Werkzeug's `generate_password_hash()` for secure password storage
   - Passwords are never stored in plain text
   - Uses `check_password_hash()` for authentication

2. **SQL Injection Protection**
   - All database queries use parameterized statements
   - Input is never directly concatenated into SQL queries

3. **Session Management**
   - Session key stored server-side
   - Secret key should be changed in production
   - Session data includes: user_id, username, full_name

4. **Input Validation**
   - Required field validation
   - Username minimum 3 characters
   - Password minimum 6 characters
   - Email format validation
   - Password confirmation matching

## Error Handling

The application provides clear error messages for:
- Missing form fields: "All fields are required"
- Duplicate username/email: "Username or email already exists"
- Invalid credentials: "Invalid username or password"
- Password mismatch: "Passwords do not match"
- Short username/password: "Username must be at least 3 characters"
- Database errors: Informative error messages

## Routes

| Route | Method | Purpose |
|-------|--------|---------|
| `/` | GET | Home (redirects to login or tracker) |
| `/signup` | GET, POST | User registration page |
| `/login` | GET, POST | User login page |
| `/tracker` | GET | Time tracker dashboard (protected) |
| `/profile` | GET | User profile page (protected) |
| `/logout` | GET | Logout and clear session |
| `/add_entry` | POST | Add a new time entry (protected) |
| `/edit_entry/<entry_id>` | GET, POST | Edit time entry (protected) |
| `/delete_entry/<entry_id>` | POST | Delete time entry (protected) |
| `/api/graph/daily` | GET | Get daily average hours for current week (protected) |
| `/api/graph/weekly` | GET | Get weekly average hours for current month (protected) |
| `/api/graph/monthly` | GET | Get monthly average hours for current year (protected) |

## Protected Routes

The following routes require authentication:
- `/tracker` - Time tracker dashboard
- `/profile` - User profile
- `/add_entry` - Add time entry
- `/edit_entry/<entry_id>` - Edit time entry
- `/delete_entry/<entry_id>` - Delete time entry
- `/api/graph/*` - All chart data endpoints

## Data Persistence

- All user and time entry data is stored in `users.db` (SQLite database)
- Database is created automatically on first run
- Data persists across server restarts
- Each entry includes date, time, hours worked, and deviation tracking
- TimeZone: Uses server local time for calculations

## Configuration

To modify app settings, edit `app.py`:

```python
# Change secret key (IMPORTANT for production)
app.secret_key = 'your-secret-key-change-this-in-production'

# Change port
app.run(debug=True, port=5001)

# Target working hours per day (in minutes)
TARGET_MINUTES = 522  # 8:42 hours
```

## Testing the Application

### Test Flow 1: Register and Login
1. Start the app
2. Go to `/signup`
3. Register a new user with details
4. Login with the same credentials
5. View tracker dashboard

### Test Flow 2: Add Time Entries
1. Login as a user
2. Go to "Time Tracker" page
3. Fill in today's in time (09:00) and out time (18:00)
4. Click "Add Entry"
5. Verify entry appears in weekly table
6. Check that total hours and deviation are calculated correctly

### Test Flow 3: View Weekly Data
1. Login as a user
2. Add multiple entries for different days in the current week
3. View the "📊 Week #X - Time Entries" table
4. Verify all entries are displayed with correct calculations
5. Use ◀ button to go to previous week
6. Verify weekly table shows correct data for that week
7. Use "This Week" button to return to current week

### Test Flow 4: View Analytics Charts
1. Login as a user
2. Ensure there are entries for the current week
3. Go to "Login Hours Analytics" section
4. View "This Week" tab - should show daily average bars
5. View "This Month" tab - should show weekly average bars
6. View "This Year" tab - should show monthly average bars
7. Hover over bars to see values in tooltip

### Test Flow 5: Edit/Delete Entries
1. Login as a user
2. Add an entry: 09:00 to 18:00
3. Click "Edit" button on the entry
4. Change out time to 17:00
5. Save changes - verify updated hours
6. Click "Delete" button
7. Verify entry is removed from table

### Test Flow 6: Month Navigation
1. Login as a user
2. Go to "Monthly Statistics" section
3. Use ◀ button to view previous month
4. Verify stats show correct month data
5. Use "This Month" button to return to current month
6. Use ▶ button to navigate to next month (should show empty if future month)

### Test Flow 7: Theme Support
1. Login as a user
2. Click the ☀️ (Light) button in header
3. Verify page switches to light theme
4. Click the 🌙 (Dark) button
5. Verify page switches to dark theme
6. Click accent color pickers
7. Select different colors
8. Verify theme preference persists after page refresh

## Key Calculations

### Total Hours Worked
```
Total Hours = (Out Time - In Time) in decimal format
Example: 09:00 to 18:00 = 9.0 hours
Example: 09:00 to 17:30 = 8.5 hours
```

### Deviation from Target
```
Target Hours = 8:42 (522 minutes)
Deviation = (Total Hours × 60) - Target Minutes
Example: 9 hours worked = 540 minutes - 522 minutes = +18 minutes
Example: 8 hours worked = 480 minutes - 522 minutes = -42 minutes
```

### Weekly Average
```
Weekly Average = Total Hours for Week / Number of Working Days
Shows average daily hours worked during the week
```

## Notes

- The database file (`users.db`) is created in the same directory as `app.py`
- For production deployment, change the `secret_key` to a random, secure value
- Target hours is set to 8:42 (522 minutes) per day by default
- All times are in 24-hour format (HH:MM)
- Dates are stored in ISO 8601 format (YYYY-MM-DD)
- For production deployments, consider enabling HTTPS and implementing rate limiting

## Troubleshooting

**Port already in use**: Change the port in `app.py`
```python
app.run(debug=True, port=5002)  # Use a different port
```

**Database errors**: Delete `users.db` and restart the app to reinitialize
```bash
rm users.db
python app.py
```

**Import errors**: Make sure dependencies are installed:
```bash
pip install -r requirements.txt
```

**Time calculations incorrect**: Verify times are in HH:MM 24-hour format

## Future Enhancements

- Multiple project/task tracking
- Time export to CSV/PDF reports
- Advanced filtering and search
- Overtime tracking and reporting
- Team Dashboard (admin view)
- Email notifications for time entry reminders
- Mobile app for time tracking
- Integration with calendar systems
- Geolocation tracking
- Biometric authentication
- Rest day management
- Leave/vacation tracking
- Performance analytics and insights
- Approval workflow for time entries

---

**Created**: February 2026  
**Technology**: Flask + SQLite + Chart.js + HTML5/CSS3  
**License**: Open Source  
**Purpose**: WorkTime Logging & Analytics Application
