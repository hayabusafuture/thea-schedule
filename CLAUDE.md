# CLAUDE.md - AI Assistant Guide

## Project Overview

**Thea's School Schedule** is a self-contained, single-page web application that displays a child's school schedule, rotating weekly menu, and 2026 holidays for "The Schoolhouse - Sweet Peas 2026" program (Singapore-based childcare/preschool).

### Key Features
- Real-time schedule with current activity highlighting
- Today/Tomorrow toggle for schedule viewing
- 5-week rotating meal menu with bilingual descriptions (English/Chinese)
- 2026 holidays and school closures list
- Mobile-responsive design

---

## Project Structure

```
thea-schedule/
├── index.html      # Single monolithic file containing all HTML, CSS, and JavaScript
├── CLAUDE.md       # This file
└── .git/           # Git repository
```

This is a **zero-dependency** project with no build tools, no npm packages, and no external libraries.

---

## Technology Stack

| Technology | Usage |
|------------|-------|
| HTML5 | Document structure and semantic markup |
| CSS3 | Styling (inline `<style>` block) |
| Vanilla JavaScript | Application logic (inline `<script>` block) |
| No frameworks | No React, Vue, Angular, etc. |
| No build tools | No webpack, Vite, etc. |
| No package manager | No npm, yarn, pnpm |

---

## File Structure Within index.html

The single `index.html` file is organized in three main sections:

| Lines | Section | Description |
|-------|---------|-------------|
| 1-6 | Head setup | DOCTYPE, meta tags, title |
| 7-562 | CSS Styles | All styling within `<style>` tag |
| 564-665 | HTML Markup | Page structure and containers |
| 667-1097 | JavaScript | Application logic within `<script>` tag |

---

## Architecture Patterns

### Data Structures

The application uses three main data objects:

1. **`menuData`** (lines 668-804): 5-week rotating menu
   ```javascript
   menuData = {
     1: {  // Week number
       monday: {
         breakfast: { name: "...", desc: "...", cn: "..." },
         lunch: { name: "...", desc: "...", cn: "..." },
         tea: { name: "...", desc: "...", cn: "..." }
       },
       // ... other days
     },
     // ... weeks 2-5
   }
   ```

2. **`scheduleData`** (lines 806-825): Daily schedule array
   ```javascript
   scheduleData = [
     { time: "7:00am", hour: 7, min: 0, activity: "Activity Name" },
     { time: "9:00am", hour: 9, min: 0, activities: { 0: "Mon", 1: "Tue", ... } },
     { time: "8:30am", hour: 8, min: 30, activity: "Eat the World", meal: "breakfast" },
     // ...
   ]
   ```

3. **`holidays`** (lines 827-851): 2026 holiday list
   ```javascript
   holidays = [
     { name: "Holiday Name", date: "1 January", day: "Thursday", type: "Public Holiday" },
     // types: "Public Holiday", "Childcare Closure", "Closes at 3pm"
   ]
   ```

### State Variables

```javascript
let currentWeek = 1;      // Selected week in menu view (1-5)
let currentDay = 0;       // Selected day in menu view (0=Mon, 4=Fri)
let viewingDay = 'today'; // 'today' or 'tomorrow' for schedule view
```

### Key Functions

| Function | Purpose |
|----------|---------|
| `getCurrentMenuWeek(date)` | Calculates which of the 5 rotating weeks applies |
| `getCurrentTimeSlot()` | Finds current schedule item based on time |
| `getCurrentMeal()` | Determines if currently at breakfast/lunch/tea time |
| `showSection(section)` | Tab navigation handler |
| `showDayView(day)` | Today/Tomorrow toggle handler |
| `showWeek(week)` | Menu week selector handler |
| `showDay(day)` | Menu day selector handler |
| `renderDaySchedule()` | Renders today's/tomorrow's schedule |
| `renderMenu()` | Renders the weekly menu view |
| `renderHolidays()` | Renders the holidays list |
| `updateTime()` | Updates header clock (runs every second) |

---

## CSS Conventions

### Color Palette
- **Primary**: `#667eea` (purple-blue gradient start)
- **Secondary**: `#764ba2` (purple gradient end)
- **Accent**: `#ff6b6b` (red - for "NOW" badge)
- **Success (Holiday)**: `#2e7d32` on `#e8f5e9`
- **Warning (Early Close)**: `#856404` on `#fff3cd`
- **Error (Closure)**: `#c00` on `#ffe0e0`

### Component Naming
```css
.card { }           /* Container cards */
.schedule-item { }  /* Schedule list items */
.meal-card { }      /* Menu meal cards */
.meal-inline { }    /* Inline meal info in schedule */
.day-toggle { }     /* Toggle button group */
.week-selector { }  /* Week selection buttons */
.day-tabs { }       /* Day selection tabs */
.holiday-item { }   /* Holiday list items */
```

### State Classes
```css
.active { }   /* Active tab/button/section */
.current { }  /* Currently active schedule item */
.past { }     /* Past schedule items (dimmed) */
```

### Responsive Breakpoints
- **600px**: Main mobile breakpoint
- **400px**: Small mobile adjustments

---

## Development Workflow

### Making Changes

1. Edit `index.html` directly
2. Refresh browser to test changes
3. No build step required

### Local Testing

Simply open `index.html` in a browser:
```bash
open index.html
# or
xdg-open index.html
# or start a local server
python3 -m http.server 8000
```

### Adding a New Menu Item

1. Locate the `menuData` object (line 668)
2. Find the correct week (1-5) and day
3. Add/modify the meal entry with `name`, `desc`, and `cn` fields

### Adding a New Holiday

1. Locate the `holidays` array (line 827)
2. Add a new object with `name`, `date`, `day`, and `type` fields
3. Valid types: `"Public Holiday"`, `"Childcare Closure"`, `"Closes at 3pm"`

### Modifying the Schedule

1. Locate the `scheduleData` array (line 806)
2. Each entry needs: `time`, `hour`, `min`, and either:
   - `activity` (same for all days), or
   - `activities` object with day indices (0=Mon, 4=Fri)
3. For meal times, include `meal: "breakfast"|"lunch"|"tea"`

---

## Code Style Guidelines

### JavaScript
- Use vanilla JavaScript (ES6+)
- No external dependencies
- Use template literals for HTML generation
- Use `innerHTML` for DOM updates
- Event handlers via inline `onclick` attributes

### CSS
- All styles in single `<style>` block
- Use flexbox for layouts
- Mobile-first responsive adjustments via media queries
- Use `linear-gradient` for branded backgrounds
- Consistent border-radius values (10px, 15px, 20px, 25px)

### HTML
- Semantic HTML5 elements
- IDs for JavaScript-manipulated elements
- Classes for styling

---

## Important Considerations

### Time Handling
- Schedule times use 24-hour format internally (`hour` field)
- Display times use 12-hour format with am/pm
- Menu week calculation uses specific date ranges defined in `getCurrentMenuWeek()`

### Week Calculation
The 5-week rotating menu follows a predefined schedule (lines 858-877). When adding new year support, update these date ranges.

### Bilingual Content
All menu items include Chinese translations in the `cn` field. Maintain this pattern for consistency.

---

## Deployment

Static file hosting - simply deploy `index.html` to any web server:
- GitHub Pages
- Netlify
- Vercel
- Any static hosting service
- Any HTTP server (Apache, Nginx, etc.)

No environment variables or server-side processing required.

---

## Git Workflow

```bash
# Current branch
git checkout claude/add-claude-documentation-UbZAe

# Commit changes
git add index.html
git commit -m "Description of changes"

# Push to remote
git push -u origin claude/add-claude-documentation-UbZAe
```

---

## Common Tasks for AI Assistants

### When Asked to Modify the Schedule
1. Read the `scheduleData` array
2. Understand the time slot structure
3. Make changes preserving the `hour` and `min` fields for time calculations

### When Asked to Add/Modify Menus
1. Read the `menuData` object
2. Maintain the nested structure: week > day > meal > {name, desc, cn}
3. Ensure Chinese translations are included

### When Asked to Update Holidays
1. Read the `holidays` array
2. Add entries with proper type classification
3. Ensure date format matches existing entries ("1 January" style)

### When Asked About Styling
1. All CSS is in lines 7-562
2. Follow existing naming conventions
3. Test responsive behavior at 600px and 400px breakpoints

### When Debugging Time-Related Issues
1. Check `getCurrentTimeSlot()` for schedule highlighting
2. Check `getCurrentMeal()` for meal time detection
3. Check `getCurrentMenuWeek()` for menu week calculation
