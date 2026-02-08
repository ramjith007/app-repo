# My Time Zone - Time Tracking Application

A simple Flask-based time tracking application for logging daily working hours with weekly and monthly summaries.

## Features

✅ **Daily Time Entry**: Simple form to log in/out times (HH:MM format)  
✅ **Automatic Calculations**: 
   - Total hours worked per day
   - Deviation from 8:42 (target working hours)
   - Weekly cumulative totals and deviations
   - Monthly totals

✅ **Data Persistence**: SQLite database stores all entries  
✅ **Clean UI**: Minimal, table-based interface focused on functionality  
✅ **Edit/Delete**: Manage entries with delete functionality  
✅ **Duplicate Prevention**: Cannot create multiple entries for same date  

## Installation

### Prerequisites
- Python 3.7+
- pip (Python package manager)

### Setup

1. Navigate to the project directory:
```bash
cd TimeWarpZone
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Run the application:
```bash
python app.py
```

4. Open your browser and go to: `http://localhost:5000`

## Usage

### Adding a Time Entry
1. Select the date (defaults to today)
2. Enter "In Time" (e.g., 09:00)
3. Enter "Out Time" (e.g., 17:40)
4. Click "Add Entry"

The app will automatically:
- Calculate total hours worked
- Calculate deviation from 8:42 target
- Display updated weekly/monthly summaries

### Understanding the Display

**Deviation from 8:42**:
- Positive numbers (green): You worked more than the target
- Negative numbers (red): You worked less than the target
- The target is 8 hours 42 minutes per day

**Weekly Summary**: Shows all entries for current week with cumulative totals

**Monthly Summary**: Shows overall hours for the current month

## Database

- Database file: `time_tracker.db` (created automatically on first run)
- Table: `time_entries` with columns:
  - date (unique)
  - in_time
  - out_time
  - total_hours
  - deviation_minutes

## Technical Stack

- **Backend**: Flask 2.3.0
- **Database**: SQLite3
- **Frontend**: HTML5 with inline CSS
- **Language**: Python 3
- **No external dependencies**: Only Flask required

## Notes

- Time format is strictly HH:MM in 24-hour format
- Application auto-detects current week and month
- All times are stored and displayed in 24-hour format
- Invalid time entries (out time before in time) are prevented
- Duplicate entries for the same date are not allowed

## Troubleshooting

**Port 5000 already in use?**
Edit `app.py` and change the port in the last line:
```python
app.run(debug=True, port=5001)  # Use different port
```

**Database issues?**
Delete `time_tracker.db` and restart the app to recreate the database.

---

Enjoy tracking your time! ⏱️
