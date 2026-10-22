# LearnLab — Business Requirements

**Business Case ID:** BC-AINE-011  
**Project:** Lightweight Learning Management System (LMS)  
**Domain:** EdTech — Online Learning

## 1. Business Overview

LearnLab is a lightweight online learning platform intended to let learners discover courses, enroll, work through lessons, complete quizzes, and track their learning progress. Instructors need to create and organize learning content and monitor learner progress. Administrators need to manage the course catalog, control course publication, and review enrollment and engagement information.

The initial release focuses on core learning-management workflows. External services such as video streaming and payment processing are not part of this release and are represented by stubs.

## 2. Business Objectives

The platform must:

- Provide a searchable catalog of available courses.
- Allow learners to enroll in published courses and access their learning content.
- Support structured learning content organized into courses, modules, and lessons.
- Provide quizzes with automatic, consistent grading.
- Track lesson completion, course progress, and quiz performance.
- Give instructors visibility into learner progress across their courses.
- Give administrators control over course publication and visibility into enrollment and engagement.
- Protect learner information and ensure users can access only the information permitted by their role.

## 3. Stakeholders and User Roles

### 3.1 Learner

A learner uses the platform to find courses, enroll, study lessons, complete quizzes, and monitor their own progress and results.

### 3.2 Instructor

An instructor creates and maintains course content, including course details, modules, lessons, and quiz questions. Instructors can review aggregated learner progress for their courses.

### 3.3 Administrator

An administrator manages the course catalog, publishes or unpublishes courses, approves courses for publication, and monitors enrollment-level analytics and engagement.

## 4. Functional Business Requirements

### 4.1 Course Catalog and Discovery

- **BR-001:** The platform shall display courses that are published and available for learner discovery.
- **BR-002:** Learners shall be able to search or filter published courses by category and difficulty.
- **BR-003:** Course information shall include a title, description, category, and difficulty.
- **BR-004:** Courses shall have a lifecycle state so their availability can be managed.
- **BR-005:** Courses shall support the lifecycle states `DRAFT`, `PUBLISHED`, `UNPUBLISHED`, and `ARCHIVED`.
- **BR-006:** A course shall begin in the `DRAFT` state when an instructor creates it.
- **BR-007:** Administrators shall be able to manage the course catalog and approve courses for publication.

### 4.2 Course Authoring and Content Structure

- **BR-008:** Instructors shall be able to create a course with a title, description, category, and difficulty.
- **BR-009:** Instructors shall be able to add modules to a draft course.
- **BR-010:** Instructors shall be able to add lessons within modules, preserving the hierarchy: course → modules → lessons.
- **BR-011:** Instructors shall be able to author quiz questions associated with learning modules.
- **BR-012:** Published course content shall not be changed directly. The course must first be unpublished before its content is modified.
- **BR-013:** Course lifecycle changes shall follow the permitted state transitions. Invalid transitions shall be rejected.

### 4.3 Course Publication and Availability

- **BR-014:** Administrators shall be able to publish a draft course.
- **BR-015:** Administrators shall be able to unpublish a published course.
- **BR-016:** The platform shall enforce the course lifecycle sequence `DRAFT → PUBLISHED → UNPUBLISHED → ARCHIVED`; invalid transitions shall be rejected.
- **BR-017:** Learners shall be able to enroll only in courses in the `PUBLISHED` state.
- **BR-018:** Unpublished or archived courses shall not be available for new learner enrollment.

### 4.4 Learner Registration and Enrollment

- **BR-019:** A learner shall be able to register for an account.
- **BR-020:** A learner shall be able to enroll in a published course.
- **BR-021:** A learner shall have no more than one enrollment for the same course. Repeated enrollment attempts shall not create duplicate enrollments.
- **BR-022:** Enrollment records shall be retained as an append-only history.
- **BR-023:** The platform shall provide suitable sample data for demonstration, including learners, instructors, courses, and sample quizzes. Only dummy data shall be used in the project.

### 4.5 Lesson Delivery and Completion

- **BR-024:** Enrolled learners shall be able to work through a course's modules and lessons in sequence.
- **BR-025:** Learners shall be able to mark a lesson as complete.
- **BR-026:** Lesson-completion records shall be retained as an append-only history.
- **BR-027:** The platform shall calculate course progress as the number of completed lessons divided by the total number of lessons in the course.
- **BR-028:** The platform shall calculate and retain a progress snapshot for each learner and course.
- **BR-029:** Learners shall be able to view their own progress percentage across their enrolled courses.

### 4.6 Quizzes and Assessment

- **BR-030:** The platform shall support multiple-choice questions with one correct answer.
- **BR-031:** The platform shall support true/false questions.
- **BR-032:** The platform shall automatically grade quiz attempts using a deterministic rule.
- **BR-033:** Quiz scores shall be expressed as integer percentages and calculated consistently.
- **BR-034:** Learners shall be allowed to attempt a quiz more than once.
- **BR-035:** The platform shall retain each quiz attempt, its timestamp, and its result as append-only history.
- **BR-036:** The platform shall record the learner's best score for each quiz.
- **BR-037:** Learners shall be able to view their quiz scores across enrolled courses.

### 4.7 Instructor Progress Monitoring

- **BR-038:** Instructors shall be able to view learner progress aggregated by course.
- **BR-039:** The aggregated view shall count learners in these progress bands: 0–25%, 25–50%, 50–75%, and 75–100%.
- **BR-040:** Instructors shall not be permitted to grade learners enrolled in their own courses.
- **BR-041:** Instructors shall not be able to view learner information outside the access allowed to their role.

### 4.8 Administration and Analytics

- **BR-042:** Administrators shall be able to manage the course catalog.
- **BR-043:** Administrators shall be able to publish and unpublish courses and approve courses for publication.
- **BR-044:** Administrators shall be able to monitor enrollment-level analytics.
- **BR-045:** Administrators shall be able to monitor learner engagement information available within the platform.

## 5. Business Rules

- **RULE-001 — Published-course enrollment:** Enrollment is permitted only when a course is `PUBLISHED`.
- **RULE-002 — Unique enrollment:** A learner may have only one enrollment per course.
- **RULE-003 — Course lifecycle:** Course states are `DRAFT`, `PUBLISHED`, `UNPUBLISHED`, and `ARCHIVED`; invalid transitions must be rejected.
- **RULE-004 — Course editing:** Published course content must not be mutated unless the course is first unpublished.
- **RULE-005 — Lesson completion:** A learner can mark lessons complete; course progress is based on completed lessons divided by total lessons.
- **RULE-006 — Quiz question types:** Supported question types are single-correct-answer multiple choice and true/false.
- **RULE-007 — Deterministic grading:** The same quiz answers and answer key must produce the same result; scores are integer percentages.
- **RULE-008 — Quiz retries:** Multiple attempts are allowed, each attempt is timestamped and retained, and the best score is recorded.
- **RULE-009 — Historical records:** Enrollment records, lesson completions, and quiz attempts are append-only.
- **RULE-010 — Role separation:** Learner, instructor, and administrator permissions must be separated.
- **RULE-011 — Learner privacy:** Learners must not be able to access another learner's progress. Learner personally identifiable information, including email, phone, and performance data, must not be written to logs in plaintext.
- **RULE-012 — Instructor restrictions:** Instructors must not grade learners enrolled in their own courses.
- **RULE-013 — Synthetic data:** Project data must be dummy data; confidential or customer-sensitive organizational data must not be used.

## 6. Business-Level Non-Functional Requirements

### 6.1 Security and Privacy

- **NFR-BR-001:** The platform shall enforce role-based access boundaries for learners, instructors, and administrators.
- **NFR-BR-002:** A learner shall only be able to access their own progress and results.
- **NFR-BR-003:** Learner personally identifiable information and performance data shall not appear in plaintext logs.

### 6.2 Data Integrity and Consistency

- **NFR-BR-004:** Enrollment, lesson-completion, and quiz-attempt histories shall be append-only.
- **NFR-BR-005:** Quiz grading and score calculations shall be deterministic and expressed as integer percentages.
- **NFR-BR-006:** Invalid course lifecycle transitions shall be prevented.
- **NFR-BR-007:** Published course content shall not be modified without an unpublish transition.

## 7. Scope Boundaries

### 7.1 Included in the Initial Release

- Course catalog browsing and search by category and difficulty.
- Course authoring and organization into modules and lessons.
- Course publication and unpublication controls.
- Learner registration and course enrollment.
- Sequential lesson learning and lesson completion tracking.
- Multiple-choice and true/false quizzes with automatic grading.
- Multiple quiz attempts, best-score retention, and attempt history.
- Learner progress and quiz-score views.
- Instructor course-level progress aggregation.
- Administrator catalog management and enrollment/engagement analytics.
- Dummy/sample data for demonstration.

### 7.2 Explicitly Out of Scope

- Real video streaming or content delivery network integration.
- Real payment gateway integration or paid-course workflows.
- Live virtual classes.
- Real proctoring or anti-cheat functionality.
- Certificate generation and external certificate verification.
- Production deployment, secret management, and multi-region operation.

## 8. Success Outcomes

The business outcomes expected from the initial release are:

- Learners can discover published courses, enroll without duplicate records, study lessons, complete quizzes, and review their progress.
- Instructors can create structured courses and review learner progress by course.
- Administrators can control course availability and monitor enrollment and engagement.
- Quiz results and course progress are calculated consistently.
- Role boundaries and learner privacy are maintained.
- The core LMS workflow is usable without relying on live video or payment integrations.

The source brief does not define numeric business KPIs or target thresholds beyond the functional rules above. Any future targets for enrollment conversion, learner completion, engagement, availability, or satisfaction should be agreed with business stakeholders rather than assumed.

## 9. Clarifications and Open Business Questions

The source brief does not specify the following details. They should be confirmed with stakeholders before being treated as requirements:

1. What exact registration fields and account-verification steps are required?
2. What should happen to existing enrollments and learner access when a course is unpublished or archived?
3. How should course progress be handled when a course has zero lessons?
4. How are quiz questions weighted, and how should unanswered questions be scored?
5. Is there a passing score, and if so, is it configured per quiz or globally?
6. Does “best score” apply per quiz, per module, or per course?
7. What exact analytics and engagement measures must administrators see?
8. Which course changes require administrator approval, and what is the approval workflow?
9. Should learners be able to leave a course or cancel an enrollment?
10. What retention or deletion policy applies to learner accounts and learning records?

These are open questions, not assumed features of the initial release.
