# Open eClass changelog

Consolidated release notes, newest first. The commit-level history is in `changelog.txt`.

## Version 4.5 (unreleased)

- **Exercises**:
  - New question type: file upload.
  - Programming exercises with a code editor and syntax highlighting.
  - New implementation of oral questions.
  - Export exercise results to PDF.
  - Improvements in the exercise form and feedback.
- **Questionnaires**: Multiple submissions with results history, with export to Excel and PDF.
- **Attendance**: Spreadsheet export and import of attendance books.
- **AI service**: Support for custom OpenAI-compatible providers.
- **Authentication**: Keycloak SSO logout URL option.
- **Course Administration**: eduAPI support for synchronizing courses and users.
- **Course Creation**: Cadmos / Coby course export integration.
- **Announcements**: Math symbols in the announcements list; select-all in bulk actions.
- **Documents**: Select-all in bulk actions.
- **Themes & UI**:
  - New themes: Business 4, Business 5, School 4, University 4, University 5.
  - Replace Select2 with SlimSelect (in progress).
- **Mobile App**: Recover expired sessions in the app WebView.
- **Reports**: Replace mPDF with the html2pdf JavaScript library.
- Remove FlowPlayer and support for FLV / SWF media.
- Remove Antivirus support from External Tools.
- Updated third-party libraries (jsTree, phpSysInfo, CodeMirror).
- Bug fixes and UI improvements.
- Security fixes.

## Version 4.4.3 (2026-10-23)

- **Questionnaires**: LimeSurvey integration checks poll visibility before launch.
- **Questionnaires, Sessions**: Various improvements.
- Docker image based on Alpine 3.24.

## Version 4.4.2 (2026-10-14)

- **Learning Analytics**: Course user analytics.
- **Mindmap**: Upgrade the jsMind library; open `.jm` files in Mindmap.
- **Exercises**: Show feedback according to user score; fixes in strict exam mode and matching questions.
- **Course Home**: "Unit completion" option in the unit action button.
- **Multimedia**: Fixes in access to and display of video files.
- **Installation & Upgrade**: Fixes in automated installation and in the `api_token` table.
- Updated translations.
- Updated third-party libraries.
- Bug fixes and UI improvements.
- Security fixes.

## Version 4.4.1 (2026-08-10)

- **Themes & UI**: Enable or disable user and course widgets from the admin settings; accessibility improvements.
- **Gradebook**: Better handling of decimal grades; empty submissions no longer become 0.
- **User Portfolio**: Hide course completion percentage from teachers.
- **Administration**: Fixes in API token and CAS authentication settings.
- Bug fixes.

## Version 4.4 (2026-07-20)

- **Multi-tenancy**:
  - Tenant options page, custom subdomains, and per-tenant title, intro text, themes and announcements.
  - Tenant administrators, certificate templates and filtered statistics.
- **Gamification**: Points games with levels, one-time and recurring activities, and an optional anonymized leaderboard.
- **Certificates & Badges**: OpenBadges backpack integration with external badge synchronization.
- **E-Portfolio**:
  - Personal files, notes, reflection comments and external badges as resources.
  - Visibility level per resource.
  - New fields for language proficiency, research profile, and social and volunteer activities.
- **Exercises**: Safe Exam Browser support, enabled globally or per course.
- **Sticky Notes**: New module with categories and drag-and-drop ordering.
- **External Repositories**: Multimedia content from external repositories (e.g. DSpace, Islandora, Pixabay), also in course units.
- **Learning Path**: New SCORM player with iframe support.
- **Authentication**: OpenID (Keycloak) login.
- **Course Settings**: Course start and end dates; course registration start and end dates.
- **Course Creation**: Coby and Cadmos integration.
- **Forum, Wall**: Suppressed words list, managed in the admin settings.
- **Administration**:
  - Delete multiple courses at once.
  - Multilingual category names and custom profile and e-portfolio fields.
  - Fetch and delete BigBlueButton recordings.
- **API**: Department-based access control; SCORM tracking scores; user search by email.
- **Themes & UI**: New themes and theme creation options, and accessibility improvements.
- Upgrade to TinyMCE 8.
- Remove Open Meetings support.
- Updated translations.
- Bug fixes and UI improvements.
- Security fixes.

## Version 4.3.3 (2026-04-03)

- Accessibility improvements.
- Bug fixes.

## Version 4.3.2 (2026-03-31)

- **Learning Path**: Upload files with Uppy; sortable modules; UI improvements in the create and edit form.
- **Attendance**: Fix an error during export to Excel.
- **Course Units**: Fix the testimonial view.
- **Administration**: Activity course type requires activity headings to be defined.
- Accessibility and UI improvements.

## Version 4.3.1 (2026-03-20)

- **Mobile App**: Update the course tools API response.
- **Course Units**: Cancel button in the edit form.
- Fixes in offline courses, multimedia playback and action buttons.

## Version 4.3 (2026-03-17)

- **Exercises**: Certainty grade in certain question types; improvements in feedback and statistics.
- **Questionnaires**:
  - New question types: Date, Date and Time, Short Answer, File Upload.
  - Mandatory answers, graded questions and sub-questions.
- **Course Units**: Improvements in UI and functionality.
- **Learning Path**: Improvements in the SCORM player.
- **External Tools**: UniFlix support.
- **Course Administration**: Import a course's content into another existing course.
- Improvements in entering math symbols in the TinyMCE editor, with a preview window.
- Users can choose their own UI theme.
- Security fixes.

## Version 4.2.1 (2025-12-30)

- **Assignments**: Fix group submissions in student view; improvements in editing rubrics.
- **Learning Path**: Fix the redirect after inserting an exercise.
- Fixes in links, questionnaires, document upload and the text editor color picker.
- Security fixes.

## Version 4.2 (2025-12-04)

- **Exercises**:
  - New question types: Ordering, Drag and Drop on text, Drag and Drop on image, Calculated, Oral.
  - New rules for exams; passing grade.
- **H5P**: Users can create H5P content.
- **AI service**: Question creation and free-text grading in Exercises; assistance in Course Creation.
- **Course Units**: Improvements in UI and functionality.
- **Assignments**: Improvements in UI and functionality.
- **Learning Path**: Improvements in running SCORM packages.
- **Course Administration**: Assign specific permissions to users.
- Security fixes.

## Version 4.1.2 (2025-07-16)

- **API**: Remove group memberships on unenrollment; fix user creation hook.
- **Installation & Upgrade**: `neutral` becomes the default theme.
- Fixes in video categories and user profile links.

## Version 4.1.1 (2025-07-03)

- **API**: New methods for course and group users; numeric role IDs when enrolling; fixes in SCORM endpoints.
- **Exercises**: Question pool filter by question type; PDF export with category headings; edit links in results.
- **Learning Path**: Overall student progress across all learning paths.
- TinyMCE autosave.
- Bug fixes in Forums, Assignments, Groups, Wall and LDAP authentication.

## Version 4.1 (2025-05-08)

- Support for LTI **1.3**.
- **Courses List**: UI improvements.
- **User Portfolio**: Improvements in calculating and caching notifications.
- **Questionnaires**: New table question type; UI fixes.
- **Attendance**: Register attendance via QR code.
- **Course Units**: Assign units to specific users or groups.
- **Course Settings**: New setting for user access to the course.
- **Learning Path**: UI improvements and better duration calculation.
- **Assignments**: Fixes in creating and editing rubrics.
- **Wall**: Fixes in displaying posts.
- **Statistics**: Improvements in admin monthly reports and caching.

## Version 4.0.3 (2025-03-11)

- **Users**: Change guest account password from the course users list.
- **Glossary**: Improvements in the add term form.
- **Administration**: Deleting a user also deletes the courses they administer.
- Fixes in course units and upgrade.

## Version 4.0.2 (2025-02-17)

- **Documents**: Bulk move of files.
- **Course Units**: UI improvements; download prevention for PDF documents.
- **Assignments**: Grading email notifications are off by default.
- **Administration**: Help links in admin tools; External Tools UI improvements.
- Bug fixes.

## Version 4.0.1 (2024-12-13)

- **Documents**: File replacement.
- **H5P**: Update h5p-core to 1.27; load assets from `vendor/`.
- Fixes in e-portfolio resources, the calendar iCal stream, user profile and wall.

## Version 4.0 (2024-11-21)

- New **user interface**.
- New themes.
- Accessibility support according to **WCAG 2.1 AA**.
- Customizable **home page**.
- Redesigned **user portfolio** and **course home**.
- Predefined **widgets**.
- **Announcements**: Bulk actions (move, delete, change visibility).
- **Documents**:
  - Bulk actions (move, delete, change visibility).
  - File upload via drag and drop.
  - Setting to prevent file download.
- **Course Units**: UI improvements.
- **Exercises**: Exam exercises.
- **H5P**: Support for new features and libraries.
- **Administration**: Reorganized tools.
- Updated third-party libraries (Bootstrap, Blade, Zip, PHP Spreadsheet).

## Version 3.16.4 (2024-11-19)

- **H5P**: Disable the H5P hub when creating new content.
- **Authentication**: User info in the Shibboleth / CAS login hook.
- Fixes in attendance, calendar events, hierarchy and statistics.

## Version 3.16.3 (2024-10-08)

- **Course Settings**: Course clone for power users and department managers.
- **Users**: No emails to users with unverified addresses when adding them to a course.
- Fixes in mail verification, assignments, groups and the learning path viewer.

## Version 3.16.2 (2024-08-08)

- Fixes in database upgrade, math expressions and certificate preview.

## Version 3.16.1 (2024-06-05)

- Fixes in assignment submission downloads, gradebook Excel output and teleconference access for external users.

## Version 3.16 (2024-05-23)

- **Teleconference**: Zoom integration via API.
- **Documents**: Bulk actions (show / hide, move, delete).
- **Exercises**:
  - New "forbid copy / paste" option.
  - Check for questions with no grade or answers set.
- **Learning Path**: Improved progress calculation for SCORM packages.
- **Authentication**:
  - OAuth2 support.
  - Map GUnet SSO schools and departments to platform categories.
- New activities for the Flipped Classroom course type.
- Security fixes.

## Version 3.15 (2024-01-23)

- New course **Reviewer** role with read-only access to course resources.
- **Exercises**: Import from **Aiken** format; import and export in **QTI** format.
- **Users**: Registration via email invitation.
- **Questionnaires**: Quick poll.
- **Documents**: Sound and video recording.
- **Certificates & Badges**:
  - Update percentage from existing course activities.
  - New criteria: assignment participation, learning path duration.
- **Teleconference**: Microsoft Teams support.
- **Announcements**: Bulk actions (delete, change visibility).
- **Learning Path**: Improved progress calculation for SCORM packages.
- **Administration**:
  - Create users from an Excel file.
  - Maintenance mode.
  - Grant or revoke a user's right to register to courses.
- Security fixes.

## Version 3.14.1 (2023-05-17)

- **H5P**: Bug fix.

## Version 3.14 (2023-05-12)

- **Learning Path**: Improved tracking in SCORM packages.
- **Teleconference**: Jitsi, Google Meet, Zoom and WebEx integration; improved participation reports.
- **Gradebook**: Improved reports; import grades from Excel.
- **Exercises**: Question feedback and exercise completion text.
- **Questionnaires**: Various improvements; better LimeSurvey connection.
- **Statistics**: Improved user reports and admin statistics.
- **User Profile**: Improvements.
- Reports are exported as Excel (.xlsx) files.
- Compatible with PHP 8.1 and 8.2.

## Version 3.13.2 (2022-11-30)

- **Course Home**: Button to enable or disable course email notifications.
- **Questionnaires**: Sortable, searchable main list.
- Administrators can log in and unlock the platform during an upgrade.
- Bug fixes.

## Version 3.13.1 (2022-10-13)

- Fixes in Learning Path and H5P.
- Installation checks for the required `intl` PHP module.
- Updated third-party libraries.

## Version 3.13 (2022-07-07)

- Database encoding changes from **utf8** to **utf8mb4**.
- **Course Units**: Access to a unit can depend on completing activities in a previous one.
- **Exercises**: New question type "Fill in from predefined answers".
- **User Profile**: Improved course ordering; favorite courses.
- **H5P**: Reuse content by moving it between courses.
- **Blog**: Hide or show a post.
- **Assignments**: Various improvements.
- **Questionnaires**: Various improvements.
- UserWay service support.
- Newer jQuery version.

## Version 3.12.6 (2022-04-19)

- **Multimedia**: Replace FlowPlayer with Video.js.
- Updated Bootstrap Datepicker.
- Bug fixes.
- Security fixes.

## Version 3.12.5 (2022-02-21)

- **Assignments**: "Users without submissions" link; improvements with peer review.
- **Teleconference**: Assign a BigBlueButton server to all courses from the UI.
- Update jQuery to 3.6.0.
- Bug fixes.

## Version 3.12.4 (2021-12-16)

- **Course Home**: Course editors always have access to the user list.
- Fix the wall module in the course menu.

## Version 3.12.3 (2021-12-06)

- **Groups**: Add multiple users to groups in categories.
- **H5P**: Allow Summary and Advent Calendar content types.
- **Administration**: New `disable_cron_jobs` option.
- Fixes in forum, gradebook, statistics and course restore.
- Security fixes.

## Version 3.12.2 (2021-11-23)

- **H5P**: Fix access rights for new subfolders when importing H5P files.
- **Administration**: List of users with the most course registrations in admin statistics.
- Fix course links in the user portfolio.
- Improvements in English messages.
- Security fixes.

## Version 3.12.1 (2021-11-04)

- **H5P**: Fix the database structure.
- **Course Restore**: Fixes in user groups and badge criteria.

## Version 3.12 (2021-10-26)

- **H5P**: Add, import and edit H5P content in courses.
- **Authentication**: Fixes and upgrades in social network login (Twitter, Facebook, Google, etc.).
- **User Rights**: Fixes in department admin rights.
- **Course Units**: H5P content and Blog as unit resources.
- **Refresh Course**: Option to hide assignments.
- **Groups**: Fixes.
- **Learning Path**: Fixes.
- Support for PHP 8.0.

## Version 3.11.2 (2021-09-10)

- **Exercises**: Recalculate totals when grading free-text questions.
- **Questionnaires**: Skip questions with no answers.
- **User Profile**: Edit the profile when authenticated by external methods.
- Bug fixes.

## Version 3.11.1 (2021-06-10)

- **Questionnaires**: Timestamps in exported answers.
- Bug fixes.

## Version 3.11 (2021-05-25)

- **Questionnaires**:
  - Multiple user submissions.
  - "Don't know / don't answer" option.
  - Administrators can clone questionnaires to all courses.
- **Exercises**:
  - Improved CSV results.
  - One question per page without navigation.
  - New score calculation for questions with negative scores.
  - Fix cloning in the question bank.
- **Assignments**: Pagination and search; rubric fixes.
- **Course Units**: Improvements in adding exercises and assignments.
- **Learning Path**: Fixes in inserting links and in SCORM / IMSCP import and export.
- **Groups**: Add multiple members by student ID or username.
- **Administration**: Search courses by teacher name.

## Version 3.10.2 (2021-01-29)

- **Administration**: IP address in action logs.
- Fixes in exercises and teleconference.

## Version 3.10.1 (2021-01-24)

- **Course Restore**: Rename question images to new IDs.
- Fixes in exercises and forum.

## Version 3.10 (2021-01-15)

- **Exercises**:
  - Dynamic question selection by difficulty or category.
  - Better question shuffling.
  - Exercise preview.
  - Better question management and results.
- **Assignments**: Rubric fixes.
- **Chat**: Fix user text not showing in some cases.
- **Statistics**: Improved reports.
- **Teleconference**: More settings; better distribution across servers.
- **Authentication**: Minor fixes.
- New `wall` course format.
- Update TinyMCE to 4.9.
- Performance: file-based caching of hierarchy and configuration.

## Version 3.9.3 (2020-09-22)

- **Assignments**: Editors can view files when the assignment is disabled.
- **Administration**: Validate email addresses in platform settings.
- Faster exercise queries when refreshing a course.
- Bug fixes.

## Version 3.9.2 (2020-07-01)

- **Assignments**: More time for Turnitin submissions.
- **Exercises**: More logging; hide the link when no questions exist.
- Bug fixes.

## Version 3.9.1 (2020-06-23)

- Fixes in assignments and exercises.

## Version 3.9 (2020-06-12)

- **Exercises**:
  - Students can continue an interrupted attempt; course admins can delete an incomplete attempt.
  - Clone the question bank to another course of the same teacher.
  - Delete unused questions from the question bank.
  - Improvements in regrading, navigation and results.
- **Assignments**:
  - Assignments accepting multiple files, including mobile photos.
  - Improvements in assigning to groups and in .zip downloads.
  - Changes in Turnitin support.
- **Groups**: Hide groups; limit users to one group per category.
- **Gradebook**: Refresh activity grades.
- **Administration**: Fix department admin rights.
- MathJax support for LaTeX syntax.
- Security fixes.

## Version 3.8.4 (2020-06-05)

- Fixes in exercises.

## Version 3.8.3 (2020-06-02)

- Preparation for Docker images.
- Fixes in assignments, exercises and teleconference.

## Version 3.8.2 (2020-05-26)

- Fixes in exercises, messages, course units and teleconference.

## Version 3.8.1 (2020-05-07)

- **Teleconference**: BigBlueButton load balancing.
- Faster exercise and course deletion queries.
- Bug fixes.
- Security fixes.

## Version 3.8 (2020-03-23)

- **Forum**: Organize messages by thread or date; file attachments.
- **Assignments**: Peer review assignments.
- **Learning Analytics**: New module.
- **Exercises**: Improvements in running exercises.
- Improvements in Questionnaires, Gradebook, Course Completion and User Portfolio.
- New themes.

## Version 3.7.6 (2020-02-03)

- Fixes in chat, exercises and teleconference.

## Version 3.7.5 (2019-10-16)

- **Chat**: colMOOC status report.
- **Search**: Course description in unit text.

## Version 3.7.4 (2019-09-24)

- Fixes in assignments and upgrade.

## Version 3.7.3 (2019-09-17)

- **Progress**: Grade-based criteria.
- Fixes in chat, forum and questionnaires.

## Version 3.7.2 (2019-06-20)

- **Course Units**: Chat and teleconference as unit resources; more resource selection options.
- Bug fixes in documents and LTI.

## Version 3.7.1 (2019-04-25)

- Minor fixes.

## Version 3.7 (2019-04-11)

- Turnitin plagiarism support.
- Download a course for offline viewing.
- **Exercises**: Improvements and bug fixes.
- **Progress**: Improvements.
- **Course Home**: Changes in the weekly course view.
- **Messages**: Forward messages.
- User consent message and privacy policy support.
- Course prerequisites.
- Course admins can add LTI tools to a course.
- Improved English messages.

## Version 3.6.7 (2019-01-29)

- Fix for embedded view.

## Version 3.6.6 (2019-01-23)

- Bug fixes in assignments, course home and BigBlueButton.
- Security fixes.

## Version 3.6.5 (2018-10-15)

- Privacy policy text option.
- **Authentication**: SSO transition with an admin tool for exception requests.
- **Administration**: Quick links on the main page.
- Fixes in offline courses.

## Version 3.6.4 (2018-05-29)

- **Offline Course**: Improvements and fixes.
- Bug fixes in the toolbox.
- Updated translations.
- Security fixes.

## Version 3.6.3 (2018-03-01)

- **Mobile App**: External authentication information in the API.
- Bug fixes.

## Version 3.6.2 (2018-01-18)

- **Course Settings**: Backup and restore certificates and badges.
- Bug fixes.

## Version 3.6.1 (2017-12-28)

First release of the 3.6 series.

- New **Gamification** module: certificates, badges and course completion criteria.
- New **Mindmap** and **E-Portfolio** modules.
- New `activity` course type.
- **Teleconference**: User attendance support.
- Plagiarism tools and electronic toolbox support.
- **Assignments**: Rubrics for grading.
- **Exercises**: Regrading and other improvements.
- Improvements in Gradebook, Calendar, logging, Search and the online theme editor.
- Parameterized course home messages.
- **Installation & Upgrade**: Improvements.
- **Administration**: Course hierarchy tool improvements; account expiration settings; landing page settings.
- Security fixes.

## Version 3.5.6 (2017-06-26)

- Fixes in exercises, assignments and forums.

## Version 3.5.5 (2017-04-06)

- **Forum**: New course setting for user notifications.
- **Statistics**: Last visits in the user profile.
- Bug fixes.

## Version 3.5.4 (2017-03-17)

- **Groups**: Allow unlimited group size; safer self-registration.
- Fixes in authentication, forums and assignments.

## Version 3.5.3 (2017-02-01)

- **Groups**: Email button for groups; group documents in the TinyMCE file picker.
- Bug fixes.

## Version 3.5.2 (2016-11-18)

- **Authentication**: CAS setting for an alternative authentication method.
- Bug fixes.

## Version 3.5.1 (2016-11-11)

First release of the 3.5 series.

- **Course Creation**: Public course code field.
- **Administration**: Strong or weak password option.
- Reliable ordering of course units, learning paths, ebooks and FAQ.
- Bug fixes.
- Security fixes.

## Version 3.4.4 (2016-09-15)

- Fixes in exercises and admin statistics.

## Version 3.4.3 (2016-09-12)

- **Authentication**: Social login buttons on the login page; automatic account creation with HybridAuth.
- Fixes in learning paths and the portal.
- Security fixes.

## Version 3.4.2 (2016-08-12)

- Minor fixes in course home, BigBlueButton, FAQ and Google Drive.

## Version 3.4.1 (2016-07-14)

- **Learning Path**: Improvements and fixes.
- **Questionnaires**: COLLES and ATTLS legends.
- Bug fixes.
- Security fixes.

## Version 3.4 (2016-06-24)

- **Teleconference**: OpenMeetings and WebConf support; assign servers to specific courses.
- **Chat**: Multiple chat rooms.

## Version 3.3.4 (2016-04-28)

- Fixes in exercises and learning path.

## Version 3.3.3 (2016-04-20)

- **Announcements**: Sticky announcements.
- Bug fixes.

## Version 3.3.2 (2016-04-01)

- **Course Home**: Course registration button.
- Fixes in statistics, authentication and exercises.
- Security fixes.

## Version 3.3.1 (2016-02-23)

- **Attendance, Gradebook**: Action logging.
- Bug fixes.

## Version 3.3 (2016-02-15)

- Upgraded user action logging and platform statistics.
- **Authentication**: Upgraded CAS, LDAP and Shibboleth; automatic course registration rules.
- **Groups**: New features.
- Improvements in Assignments, Exercises and Questionnaires.
- **Administration**: Improvements.
- Accessibility improvements (WCAG 2.0 AA).

## Version 3.2 (2015-10-22)

- **External Tools**: Google Analytics, Antivirus, Firewall, Auto Judge, Google Drive, Dropbox, OneDrive and more.
- **Authentication**: New methods through the HybridAuth library; custom profile fields at registration.
- **Groups**: Multiple improvements; settings are now per group.
- Abuse reports.
- Improvements in user requests for closed courses.
- **Course Home**: Layout improvements.
- Improvements in Gradebook, Attendance, Assignments, Exercises, Questionnaires and admin tools.
- Improved email message format.
- Accessibility improvements.

## Version 3.1.6 (2015-07-24)

- Improvements in Gradebook, Administration, Links and Assignments.
- CSRF tokens and indirect references.
- Bug fixes.

## Version 3.1.2 (2015-06-11)

- Fixes in administration, assignments and learning path.

## Version 3.1.1 (2015-06-08)

- **Exercises**: New options for when results are shown.
- **Administration**: Rich text editor in admin email.
- Bug fixes.

## Version 3.1 (2015-05-29)

- Course completion certificates and badges.
- User blog.
- Improvements in Attendance, Gradebook, Exercises, Assignments, Ebook and Questionnaires.
- **Administration**: Improvements.
- Accessibility improvements (WCAG 2.0).
- New and enhanced themes.

## Version 3.0 (2015-04-01)

- Redesigned user portfolio and user interfaces.
- New **Gradebook**, **Attendance** and **Course Blog** modules.
- BigBlueButton integration.
- Web 2.0 tools.
- OAI-PMH support.
- New search engine.
- New hierarchical course structure.
- Course metadata according to the ECTS-CDS standard.
- Upgraded assessment, communication and collaboration tools.

## Version 2.11

- **Questionnaires**: Anonymous questionnaires, cloning, reset and CSV export of results.
- **Multimedia**: Resource categories.
- **Exercises**: Student ID; original text formatting in questions.
- **Groups**: Student ID.
- Course citation support.
- Improvements in the Android and iOS apps and the mobile API.
- General UI improvements.

## Version 2.10

- **Documents**: Linked files; create HTML documents inside the course.
- Pagination and search in mailing lists, announcements, courses and users.
- IMS CP support.
- Improvements in Course Information, Messages, Exercises, Questionnaires and Assignments.
- Android app; improvements in the iOS app and the mobile API.

## Version 2.9 (2014-03-19)

- Improvements in Dropbox, Exercises, course registration and Learning Path.
- **Authentication**: Improvements with an external database.
- Changes in directory structure.
- Improvements in the mobile API.

## Version 2.8.3 (2013-12-06)

- **Course Metadata**: Course review; performance improvements.
- Bug fixes.

## Version 2.8 (2013-11-12)

- **Dropbox**: Message exchange between users.
- **Administration**: Bulk account deletion; multiple course creation.
- **Multimedia**: Improvements.
- Course metadata (CDM, ECTS) and course licenses.
- Documents viewable only in registered courses.
- Optional password for open courses.
- GUnet Open Courses support and a new Open Course Reviewer role.

## Version 2.7 (2013-05-08)

- New **Common Documents** module.
- **Administration**: Bulk account creation.
- Second email address for student accounts.
- Improvements in Assignments and Exercises.

## Version 2.6 (2012-10-31)

- Improved course registration.
- File upload whitelists, global and per user.
- **Administration**: Merge user accounts; multi-user deletion with criteria.
- Settings for student and teacher account registration and requests.
- **Search**: Can be enabled or disabled.
- **Dropbox**: Teachers can preview course files.
- Remove the phpMyAdmin tool.
- Italian and French translations.
- Security fixes.

## Version 2.5.6 (2012-09-04)

- Minor fixes in exercises, questionnaires and announcements.

## Version 2.5.5 (2012-08-01)

- Minor fixes in assignments and the mobile API.

## Version 2.5.4 (2012-07-17)

- Minor fix in forums.

## Version 2.5.3 (2012-07-13)

- **Authentication**: Transparent transition to bcrypt passwords.
- Security fixes.

## Version 2.5.2 (2012-06-26)

- **Assignments**: Email notifications on grading; improvements for groups.
- **Mobile App**: API improvements.
- Bug fixes.
- Security fixes.

## Version 2.5.1 (2012-03-16)

- **Users**: Unregister link in user administration.
- More TinyMCE tools; video improvements in documents.
- Bug fixes.
- Security fixes.

## Version 2.5 (2012-01-04)

- New administrator roles (course and user administrator, user administrator) and an assistant professor course role.
- Users can opt out of course emails.
- Email verification during registration.
- **Assignments**: Deadlines with time and date.
- **Exercises**: Exercises with existing random questions.
- **Multimedia**: Videos in a popup window.
- New `ocean` theme.
- German translation.
- Improved online help.

## Version 2.4.1 (2011-07-21)

- **Authentication**: Fixes for alternative methods; automatic registration or request when registration is closed.
- Administrators can change a user's authentication method.
- Minor fixes in glossary and video.

## Version 2.4 (2011-06-30)

- New **Ebook** and **Glossary** modules.
- New template and themes, including `modern`, and theme administration.
- New login page.
- RSS feeds for announcements.
- Replace the Xinha editor with TinyMCE.
- **Documents, Groups, Forums**: Various changes.
- **Documents**: Download directories as .zip.
- **Groups**: Users can join multiple groups.
- **Course Units**: Display ebooks.
- **Search**: Changes.
- Public user profile.
- **Authentication**: CAS support; better Shibboleth support.
- Works with PHP 5.3, without `register_globals` and `short_open_tags`, and in MySQL strict mode.
- Improvements in admin tools, the installation wizard and online help.
- Security changes.

## Version 2.3.2 (2010-04-27)

- **Announcements**: RSS support.
- Fixes in course description and documents.

## Version 2.3.1 (2010-04-19)

- **Learning Path**: Improvements and fixes.
- New manuals.
- Spanish enabled by default.
- Bug fixes.

## Version 2.3 (2010-03-18)

- **Authentication**: Shibboleth support.
- **Course Home**: Student view.
- **Forum**: Message notifications.
- Math symbols in Exercises and Forum.
- **Administration**: Various changes and fixes.
- Optional password when creating a course.

## Version 2.2.1 (2009-12-15)

- BetaCMS bridge.
- **Course Settings**: Course units in backup and restore.
- **Exercises**: Student ID in results.
- Bug fixes.

## Version 2.2 (2009-09-17)

- New **Course Units** module: organize a course by learning resources.
- **Learning Path**: Various fixes.
- **Exercises**: Decimal grades; option to show correct answers; fix question ordering.
- **Groups**: Statistics.
- **User Profile**: Time spent per course.
- Spanish language support.

## Version 2.1.4 (2009-05-13)

- **Forum**: Topic pagination.
- Fixes in installation, upgrade and assignments.

## Version 2.1.3 (2009-03-26)

- **Announcements**: Emails sent per 50 users; optional carbon copy.
- **Forum**: Pagination.
- **Documents**: Show the title when defined.
- **Dropbox**: Encoded user files.
- **Agenda**: Visibility field.
- Keep users logged in when they try to access a closed course.
- New `config` table in the main database.
- Fixes in Learning Path, Assignments, Users, the template engine and admin tools.

## Version 2.1.2 (2008-11-14)

- HTML editor in Create Course and Course Administration.
- Fixes in user authentication and upgrade.

## Version 2.1.1 (2008-10-24)

- **Administration**: Log in as a different user.
- Fixes in statistics, course search, LDAP authentication, password recovery and upgrade.

## Version 2.1 (2008-09-08)

- New graphical user interface.
- New **Learning Path** module, compatible with SCORM 1.2 and 2004.
- New **Questionnaires** and **Wiki** modules.
- Improved Assignments and Exercises.
- Personalized user portfolio.
- Multilingual support (UTF-8).
- Math symbols.
- New embedded HTML editor.
- **Authentication**: LDAP, POP3, IMAP and external MySQL database.
- New course creation wizard.
- User auditing and statistics.
- Enhanced search.
- Improved security and new password encryption.
- New platform web portal.

## Version 2.0 (2007-05-02)

- Improved functionality and look of the platform.
- Personalized services and interfaces.
- New **Learning Path** module, compatible with SCORM 2004.
- New **Polls** and **Questionnaires** modules.
- New **Teleconference** module.
- Improved self-assessment Exercises and Assignments.
- Improved admin tool and platform registration.
- New communication tools between teachers and students.
- New course creation wizard with metadata.
- User activity logging and statistics.
- Search by various criteria.
- Security improvements.

## Version 1.7

- New directory structure: modules in `modules`, course material in `courses`, configuration in `config`.
- Improved installation and upgrade wizards.
- Interface changes using style sheets.
- New module for student-teacher communication.
- Open registration or registration by request.
- New admin and registration tools.
- **Documents**: New "view" function.
- HTTPS support.
- Improved course restore, online help and user portfolio.
- New manuals.
- Compatible with MySQL 5.x.
- New phpMyAdmin version (2.10.0.2).
- Security improvements.

## Version 1.6 (2006-04-13)

- New **File Exchange** module between teachers and students.
- **Links**: Categories.
- New HTMLArea rich text editor.
- Users can delete their own accounts when not registered in any course.
- **Users**: List export to CSV or Excel.
- **Administration**: Change course quota.
- Improvements in Agenda and admin tools.
- Compatible with newer PHP and MySQL versions.

## Version 1.5

- New **Recorded Lectures** module with links to a Video on Demand server.
- "My calendar" and "My announcements" across all registered courses.
- **Documents**: Save function.
- **Refresh Course**: Prepare an existing course for new groups of students.
- **Users**: Student email addresses visible only to the course teacher.
- Improvements in online help, admin tool interfaces and registration.
- New phpMyAdmin version (2.5.7-pl1).

## Version 1.4

- Per-course quotas for Documents, Video and Groups.
- Rich text editor in Announcements, Introductory Text, Agenda, Course Description and Assignments.
- Compatible with MySQL 4.x.
- Online help in various modules.
- Improvements in Assignments and Chat.
- LaTeX math symbols in Exercises, Forum and Introductory Text.
- Improved course list and course registration.
- Fix in Exercises.

## Version 1.3

- New **Assignments** module with deadlines, downloads and grading.
- New self-assessment **Exercises** module: multiple choice, matching, fill in the blanks.
- New course backup and restore.
- Bulk user registration from a file.
- List of schools and departments on the home page, leading to their courses.
- Minor fixes in the admin tool.
