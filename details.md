### Mobile User Journeys

#### Dental Practice Mobile Journey
**Objective**: Enable practices to manage shifts, hire professionals, and track finances on mobile (Flutter app, iOS/Android).

1. **Onboarding (Sign Up & Profile Creation)**:
   - **Flow**: Download app, select “Dental Practice,” input name, address (PostGIS-validated), license, and upload tax ID/business license via camera/drag-and-drop. Set up 2FA (Azure AD B2C, SMS/email). Submit; receive push notification: “Profile submitted for verification.”
   - **UX Details**: Multi-step form with progress bar, real-time address validation, document quality checker, HIPAA compliance tooltips.
   - **Technical Notes**: Data encrypted with TLS 1.3 (transit) and PostgreSQL TDE (rest); Azure AD B2C for 2FA (Section 3.5).

2. **Profile Approval**:
   - **Flow**: View “Pending Verification” on home screen. Receive push notification post-admin review: “Profile approved” or “Resubmit [document].” Approved users unlock full app access.
   - **UX Details**: Status badge with clickable admin comments, resubmission form, in-app support chat.
   - **Technical Notes**: Verification logged in Azure Monitor (7-year retention, Section 3.5.3).

3. **Posting a Shift**:
   - **Flow**: Tap “Post Shift,” select template (e.g., “Weekend Hygienist”) or input title, certifications, location, pay rate, timing. Submit; receive push notification: “Shift posted.”
   - **UX Details**: Template library, map-based location picker, pay rate slider with market benchmarks, save-as-template option.
   - **Technical Notes**: Shift data stored in PostgreSQL with pgvector; API (ASP.NET Core 9) ensures <300ms latency (Section 3.2).

4. **Viewing Applications**:
   - **Flow**: Tap “Applications” to view/filter applicants by certifications, ratings, proximity (PostGIS). View profiles/resumes. Receive push notification: “New applicant.”
   - **UX Details**: Swipeable applicant cards, AI match score (85% accuracy), bookmark feature, filter presets (e.g., “Top-Rated”).
   - **Technical Notes**: AI matching via pgvector/HNSW; data encrypted (Section 3.4).

5. **Selecting a Professional**:
   - **Flow**: Select applicant, view fee breakdown, confirm; payment deducted into escrow (Stripe). Professional notified; coordinate via in-app chat (Azure Web PubSub).
   - **UX Details**: Transparent cost modal, embedded chat with templates, double-tap confirmation.
   - **Technical Notes**: Payments via Dapr + Stripe Webhook; chat encrypted with TLS 1.3 (Section 3.2).

6. **Shift Completion**:
   - **Flow**: Receive 24-hour reminder notification. Mark shift complete; escrow releases payment. Rate professional (1-5 stars). Receive notification: “Payment released.”
   - **UX Details**: “Complete Shift” button, star slider for rating, review text input, payment summary.
   - **Technical Notes**: Payment release via Stripe; ratings logged in PostgreSQL (Section 2.4).

7. **Managing Activities & Calendar**:
   - **Flow**: View “Calendar” tab for shifts (pending, confirmed, completed). Access “Financials” tab for earnings, commissions, and exportable reports (CSV/PDF).
   - **UX Details**: Color-coded calendar, bar charts for commissions, export button with date filters, offline mode for recent data.
   - **Technical Notes**: Reports via Azure Functions; data encrypted (addressing gap, Section 3.5.2).

8. **Canceling a Shift**:
   - **Flow**: Select shift, tap “Cancel,” view penalty (e.g., 50% pay). Confirm; fee deducted, professional notified.
   - **UX Details**: Warning modal with penalty details, checkbox confirmation, updated earnings view.
   - **Technical Notes**: Penalties via Dapr + Stripe; notifications via Azure Web PubSub (Section 3.2).

#### Dental Professional Mobile Journey
**Objective**: Enable professionals to find jobs, apply, complete shifts, and manage earnings on mobile.

1. **Onboarding (Sign Up & Profile Creation)**:
   - **Flow**: Select “Dental Professional,” input certifications, availability, hourly rate, upload resume/license. Set up 2FA. Submit; receive notification: “Profile submitted.”
   - **UX Details**: Dropdowns for certifications, availability calendar, camera-based upload, progress tracker.
   - **Technical Notes**: Data encrypted; 2FA via Azure AD B2C (Section 3.5).

2. **Profile Approval**:
   - **Flow**: View “Pending” status. Receive notification: “Profile approved” or “Resubmit.” Approved users unlock job browsing.
   - **UX Details**: Status badge, resubmission form, support chat.
   - **Technical Notes**: Verification logged in Azure Monitor (Section 3.5.3).

3. **Browsing & Applying for Jobs**:
   - **Flow**: Tap “Jobs,” filter by location, pay, specialty. View AI-recommended jobs (85% accuracy). Apply with note (e.g., “Available weekends”). Receive notification: “Application submitted.”
   - **UX Details**: Job cards with match score, filter presets, one-tap apply, note text field.
   - **Technical Notes**: AI via ONNX Runtime 1.16; PostGIS for location queries (Section 3.4).

4. **Selection Notification**:
   - **Flow**: Receive notification: “Selected for [Shift].” Confirm via in-app chat. Receive shift details and reminder.
   - **UX Details**: Chat with pinned shift details, calendar sync, notification settings.
   - **Technical Notes**: Notifications via Azure Web PubSub; chat encrypted (Section 3.2).

5. **Shift Completion & Payment**:
   - **Flow**: Mark shift complete; payment released. Rate practice. Receive notification: “Payment released.”
   - **UX Details**: Completion button, star slider, payment confirmation with transaction ID.
   - **Technical Notes**: Payments via Dapr + Stripe; ratings in PostgreSQL (Section 2.4).

6. **Managing Activities & Calendar**:
   - **Flow**: View “Calendar” for shifts. Access “Earnings History” for payments, export as CSV/PDF.
   - **UX Details**: Color-coded calendar, line graph for earnings, export button, offline mode.
   - **Technical Notes**: Reports via Azure Functions; data encrypted (addressing gap, Section 3.5.2).

7. **Canceling a Shift**:
   - **Flow**: Select shift, tap “Cancel,” view 72-hour restriction. Confirm; receive notification: “Shift canceled, restriction applied.”
   - **UX Details**: Warning modal with countdown, appeal option via chat.
   - **Technical Notes**: Restrictions via API; notifications via Azure Web PubSub (Section 3.2).

### Web User Journeys

#### Dental Practice Web Journey
**Objective**: Provide a comprehensive dashboard for shift management, applicant screening, and financial tracking (React, Azure Static Web Apps).

1. **Onboarding (Sign Up & Profile Creation)**:
   - **Flow**: Access tempii.com, click “Sign Up as Practice,” input details, upload documents. Set up 2FA. Submit; receive email: “Profile submitted.”
   - **UX Details**: Multi-step form, map-based address picker, drag-and-drop upload, guided tour.
   - **Technical Notes**: Data encrypted; 2FA via Azure AD B2C (Section 3.5).

2. **Profile Approval**:
   - **Flow**: Dashboard shows “Pending.” Receive email: “Profile approved” or “Resubmit.” Approved users unlock dashboard.
   - **UX Details**: Status banner, resubmission form, live chat support.
   - **Technical Notes**: Verification logged in Azure Monitor (Section 3.5.3).

3. **Posting a Shift**:
   - **Flow**: Access “Shift Wizard,” select template or input details. Post; receive email: “Shift posted.”
   - **UX Details**: Table view of shifts, template library, pay rate calculator, map widget.
   - **Technical Notes**: Shift data in PostgreSQL; API ensures <300ms latency (Section 3.2).

4. **Viewing Applications**:
   - **Flow**: Access “Applicants” tab, filter by certifications/ratings/proximity. View profiles in side panel. Receive email: “New applicant.”
   - **UX Details**: Filterable table, AI match score, downloadable resumes, bookmark feature.
   - **Technical Notes**: AI via pgvector/HNSW; data encrypted (Section 3.4).

5. **Selecting a Professional**:
   - **Flow**: Select applicant, view fee breakdown, confirm; payment deducted. Coordinate via chat.
   - **UX Details**: Cost modal, embedded chat, double-click confirmation.
   - **Technical Notes**: Payments via Dapr + Stripe; chat via Azure Web PubSub (Section 3.2).

6. **Shift Completion**:
   - **Flow**: Receive email reminder. Mark shift complete; payment released. Rate professional.
   - **UX Details**: “Complete Shift” button, rating form, payment summary.
   - **Technical Notes**: Payments via Stripe; ratings in PostgreSQL (Section 2.4).

7. **Managing Activities & Calendar**:
   - **Flow**: View “Calendar” for shifts. Access “Financials” for earnings, commissions, exportable reports.
   - **UX Details**: Interactive calendar, charts for commissions, export wizard, customizable widgets.
   - **Technical Notes**: Reports via Azure Functions; data encrypted (addressing gap, Section 3.5.2).

8. **Canceling a Shift**:
   - **Flow**: Select shift, click “Cancel,” view penalty. Confirm; professional notified.
   - **UX Details**: Penalty modal, checkbox confirmation, updated earnings.
   - **Technical Notes**: Penalties via Dapr + Stripe; notifications via Azure Web PubSub (Section 3.2).

#### Admin Web Journey (Web-Only)
**Objective**: Manage platform operations, compliance, and analytics via a web dashboard.

1. **Profile Verification**:
   - **Flow**: Log in (Azure AD B2C, 2FA), access “Verification Queue,” review documents, approve/reject with comments. Users notified.
   - **UX Details**: Table with filters (user type, status), zoomable document viewer, bulk approve/reject, audit trail.
   - **Technical Notes**: Documents in Azure Storage; logged in Azure Monitor (Section 3.5).

2. **User Management**:
   - **Flow**: Access “Users” tab, search/filter users, suspend accounts, reset passwords. View activity logs.
   - **UX Details**: Searchable table, action buttons (suspend/reset), log viewer, CSV export.
   - **Technical Notes**: Azure AD B2C for access control; logged in Azure Monitor (addressing gap, Section 3.5.1).

3. **Dispute Resolution & Escrow Oversight**:
   - **Flow**: Access “Disputes” tab, review escalated issues, mediate via chat, apply refunds/penalties. Monitor escrow in “Payments” tab; flag failed transactions.
   - **UX Details**: Timeline view, chat widget, transaction table, penalty calculator.
   - **Technical Notes**: Payments via Dapr + Stripe; chat via Azure Web PubSub (Section 3.2).

4. **Analytics & Reporting**:
   - **Flow**: View “Analytics” tab for KPIs (fill rate, retention, revenue). Generate/export reports (CSV/PDF).
   - **UX Details**: Interactive charts, drill-down filters, export wizard, summary widget.
   - **Technical Notes**: Data via Azure Monitor; reports via Azure Functions (addressing gap, Section 8.2).

5. **Compliance Monitoring**:
   - **Flow**: Access “Compliance” tab, review audit logs, verify encryption, run automated checks. Export compliance reports.
   - **UX Details**: Compliance score, log viewer, export button, alert banner.
   - **Technical Notes**: Azure Policy for checks; logged in Azure Monitor (Section 8.4).

6. **System Health Monitoring**:
   - **Flow**: View “System Health” tab for API latency, payment status, container metrics. Receive dashboard alerts (PagerDuty/Teams).
   - **UX Details**: Real-time graphs, alert history, failover button.
   - **Technical Notes**: Monitoring via Azure Monitor; multi-zone deployment (Section 8.2).

7. **Enforce Cancellation Penalties**:
   - **Flow**: Access “Cancellations” tab, review disputes, apply penalties (e.g., fee deduction), override for emergencies.
   - **UX Details**: Cancellation table, evidence upload, override form, notification log.
   - **Technical Notes**: Penalties via Dapr + Stripe; logged in Azure Monitor (addressing gap, Section 3.2).

8. **Monitor Restrictions**:
   - **Flow**: View “Restrictions” tab for 72-hour lockouts, monitor compliance, override if needed.
   - **UX Details**: Table with countdown timers, filter by reason, override button.
   - **Technical Notes**: Restrictions via API; logged in Azure Monitor (addressing gap, Section 3.2).
