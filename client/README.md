# Learning Management System (LMS) - Client

A modern, full-featured Learning Management System built with React, Vite, and Tailwind CSS. This platform enables educators to create and manage courses while allowing students to browse, enroll, and learn from course content with integrated YouTube video lectures.

## 📋 Project Overview

This LMS client is a comprehensive educational platform designed with a dual-role architecture:

- **Student Portal**: Browse courses, search, enroll, view course details, watch lectures, and rate courses
- **Educator Portal**: Manage courses, track student enrollments, view analytics, and create course content

The application features a responsive design, real-time course filtering, YouTube video integration, and user authentication via Clerk.

## 🏗️ Architecture & Technology Stack

### Core Technologies
- **Framework**: React 19.1.0 with Vite 6.3.5
- **Routing**: React Router v7.6.2
- **Styling**: Tailwind CSS 3.4.17 + PostCSS
- **Authentication**: Clerk React (@clerk/clerk-react ^5.31.9)
- **UI Components**:
  - `react-youtube` - YouTube video player integration
  - `react-simple-star-rating` - Course rating system
  - `rc-progress` - Progress bar visualization
- **Utilities**:
  - `humanize-duration` - Format duration values (e.g., "2h 30m")
  - `uniqid` - Generate unique IDs for components
  - `quill` - Rich text editor for course descriptions
- **Development**: ESLint for code quality

### Project Structure
```
src/
├── App.jsx                          # Main routing configuration
├── main.jsx                         # Application entry point with Clerk & Router setup
├── index.css                        # Global styles
├── assets/
│   ├── assets.js                    # Dummy data & asset imports
│   ├── rich-text-css.txt            # Quill editor styles
│   └── [logos, icons, images]       # Visual assets
├── components/
│   ├── student/
│   │   ├── Navbar.jsx               # Navigation bar for students
│   │   ├── Hero.jsx                 # Landing page hero section
│   │   ├── Companies.jsx            # Partner/company logos showcase
│   │   ├── CoursesSection.jsx       # Featured courses carousel
│   │   ├── CourseCard.jsx           # Individual course card component
│   │   ├── CoursesListSearch.jsx    # Search bar for course filtering
│   │   ├── SearchBar.jsx            # Search functionality
│   │   ├── TestimonialsSection.jsx  # Student testimonials
│   │   ├── Rating.jsx               # Star rating component
│   │   ├── CallToAction.jsx         # CTA section
│   │   ├── Footer.jsx               # Footer navigation
│   │   └── Loading.jsx              # Loading/skeleton state
│   └── educator/
│       ├── Navbar.jsx               # Educator navigation
│       ├── Sidebar.jsx              # Sidebar menu for educator dashboard
│       └── Footer.jsx               # Educator footer
├── context/
│   └── AppContext.jsx               # Global state management (courses, user, etc.)
└── pages/
    ├── student/
    │   ├── Home.jsx                 # Landing page (Hero, Companies, Courses, Testimonials)
    │   ├── CoursesList.jsx          # Browse all courses with search filtering
    │   ├── CourseDetails.jsx        # Single course details page
    │   ├── MyEnrollments.jsx        # Student's enrolled courses
    │   └── Player.jsx               # Lecture video player
    └── educator/
        ├── Educator.jsx             # Main educator layout (nested routes)
        ├── Dashboard.jsx            # Overview of earnings, enrollments, courses
        ├── AddCourse.jsx            # Create new course form (in development)
        ├── MyCourses.jsx            # List educator's courses
        └── StudentsEnrolled.jsx     # Track student enrollments per course
```

## 🎯 Key Features

### Student Features
1. **Course Discovery**
   - Browse all available courses
   - Search courses by title/keywords
   - Filter and view course details
   - See course pricing, discounts, duration, and ratings

2. **Course Engagement**
   - View detailed course information with rich HTML descriptions
   - See all chapters and lectures organized hierarchically
   - Access free preview lectures
   - Watch lessons via integrated YouTube video player
   - Track chapter duration and total course time

3. **User Interaction**
   - Rate courses (1-5 stars) with instant feedback
   - View course ratings and student enrollment numbers
   - Enroll in courses
   - Track personal course enrollments

4. **UI/UX**
   - Responsive design for mobile, tablet, and desktop
   - Loading states during data fetch
   - Intuitive navigation
   - Company logos showcase (marketing)
   - Testimonials section

### Educator Features
1. **Course Management**
   - Create new courses (AddCourse component)
   - View all published courses
   - Manage course visibility/publishing status

2. **Analytics Dashboard**
   - Total earnings tracking
   - Recent student enrollments overview
   - Number of active courses
   - Student enrollment details per course

3. **Student Management**
   - View all students enrolled in courses
   - Track enrollment dates
   - Monitor course-specific enrollments
   - Student profile and image integration

## 📊 Global State Management (AppContext)

The `AppContext.jsx` provides centralized state for:

```javascript
{
  currency,                           // Currency symbol/code
  allCourses,                         // Array of all available courses
  enrolledCourses,                    // User's enrolled courses
  isEducator,                         // Boolean: is user an educator?
  navigate,                           // React Router navigate function
  calculateRating(course),            // Avg rating from course.courseRatings
  calculateChapterTime(chapter),      // Format chapter duration
  calculateCourseDuration(course),    // Format total course duration
  calculateNoOfLectures(course),      // Count total lectures
  fetchAllCourses(),                  // Load courses (currently dummy data)
  fetchUserEnrolledCourses()          // Load user enrollments
}
```

## 📚 Data Model

### Course Object Structure
```javascript
{
  _id: string,
  courseTitle: string,
  courseDescription: string,          // HTML formatted
  coursePrice: number,
  discount: number,                   // Percentage
  courseThumbnail: string,            // YouTube thumbnail URL
  isPublished: boolean,
  educator: string,                   // Educator ID
  enrolledStudents: [string],         // Array of student IDs
  courseRatings: [
    {
      userId: string,
      rating: number,                 // 1-5
      _id: string
    }
  ],
  courseContent: [
    {
      chapterId: string,
      chapterOrder: number,
      chapterTitle: string,
      chapterContent: [
        {
          lectureId: string,
          lectureTitle: string,
          lectureDuration: number,    // In minutes
          lectureUrl: string,         // YouTube URL
          isPreviewFree: boolean,
          lectureOrder: number
        }
      ]
    }
  ],
  createdAt: string,
  updatedAt: string
}
```

## 🛣️ Routing Structure

| Route | Component | Purpose |
|-------|-----------|---------|
| `/` | Home | Landing page with hero, companies, featured courses, testimonials |
| `/course-list` | CoursesList | All courses with search/filtering |
| `/course-list/:input` | CoursesList | Search results for input keyword |
| `/course/:id` | CourseDetails | Single course details, chapters, and lectures |
| `/my-enrollments` | MyEnrollments | User's enrolled courses |
| `/player/:courseId` | Player | Video player for watching lectures |
| `/loading/:path` | Loading | Loading skeleton for navigation transitions |
| `/educator` | Educator | Educator dashboard layout |
| `/educator/educator` | Dashboard | Earnings, enrollments, course stats |
| `/educator/add-course` | AddCourse | Create new course (WIP) |
| `/educator/my-course` | MyCourses | Educator's published courses |
| `/educator/student-enrolled` | StudentsEnrolled | Enrollments per course |

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Set up environment variables
# Create a .env.local file with:
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_CURRENCY=$ (or your preferred currency)
```

### Development

```bash
# Start development server
npm run dev

# The app will be available at http://localhost:5173 (default Vite port)
```

### Build for Production

```bash
# Build the project
npm run build

# Preview production build
npm run preview
```

### Code Quality

```bash
# Run ESLint to check code quality
npm run lint
```

## 🎨 Styling Approach

- **Tailwind CSS**: Utility-first CSS framework for responsive design
- **Custom Extensions** in `tailwind.config.js`:
  - Custom font sizes for headings
  - Grid templates for auto-fit layouts
  - Custom shadows for card components
  - Spacing utilities for sections
- **PostCSS**: Autoprefixer for browser compatibility

## 🔐 Authentication

The application uses **Clerk** for authentication:
- Manages user sign-up/sign-in
- Provides user context throughout the app
- Seamless integration with AppContext for educator role detection
- Protected routes based on authentication state

## 📦 Current Dummy Data

The project currently uses `dummyCourses` from `assets.js` containing:
- 8+ pre-configured courses
- Real YouTube video links
- Complete chapter and lecture structures
- Student enrollments and ratings
- Educator dashboard data

## 🔮 Future Enhancements / Work in Progress

1. **AddCourse Component**: Implement course creation form with:
   - Rich HTML editor for descriptions (Quill integration ready)
   - Chapter and lecture management
   - Video URL input
   - Pricing and discount settings

2. **Backend Integration**:
   - Replace dummy data with API calls
   - Real course CRUD operations
   - Student enrollment API
   - Rating submission API
   - User profile management

3. **Player Component**: Full video player implementation
   - Chapter navigation
   - Progress tracking
   - Playback controls
   - Lecture completion tracking

4. **MyEnrollments Component**: Display user courses with:
   - Progress bars per course
   - Last watched lecture
   - Continue learning links

5. **Advanced Features**:
   - Course completion certificates
   - Discussion forums
   - Live instructor sessions
   - Downloadable resources
   - Course recommendations
   - Wishlist functionality

## 🛠️ Development Workflow

The project is built with modern development practices:

- **Vite** for ultra-fast build and development
- **Hot Module Replacement (HMR)** for instant updates
- **React Strict Mode** to catch development issues
- **ESLint** for code consistency
- **Responsive Design** optimized for all screen sizes

## 📝 Configuration Files

- **vite.config.js**: Vite build configuration with React plugin
- **tailwind.config.js**: Custom Tailwind styling configuration
- **postcss.config.js**: PostCSS plugins (tailwind, autoprefixer)
- **eslint.config.js**: ESLint rules for code quality
- **package.json**: Dependencies and scripts

## 🎓 Learning Outcomes

This project demonstrates:
- Modern React patterns (Context API, Hooks)
- Component composition and reusability
- State management without Redux
- Responsive CSS with Tailwind
- Third-party integrations (Clerk, YouTube, Quill)
- Routing with React Router
- Build tool optimization (Vite)
- Professional project structure

## 📞 Support & Contact

For issues or questions about this project, refer to the component documentation or code comments throughout the application.

---

**Built with ❤️ using React and Vite**
