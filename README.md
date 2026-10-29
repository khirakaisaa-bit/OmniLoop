# Omniloop

**An AI system that lifts teachers' daily load: it sends tasks, gives feedback and turns students' input into better lessons.**

**Live demo (no install, no sign-in):** https://khirakaisaa-bit.github.io/OmniLoop/

## The problem
Learners keep asking their teacher for something to practise between live sessions, because progress comes from daily immersion, not one lesson a week. But sending tasks, chasing replies, correcting answers and spotting who is struggling by hand is too much for one teacher, so daily practice fades and the live lesson is planned without knowing what learners found difficult.

## What Omniloop does
- **Course-based daily tasks.** Each course week has an objective. It is split into four small daily tasks, plus a cheat day (catch up), a rest day, and the live session.
- **Mood-based length.** Students check in with how they feel and choose a 2-minute or a 10-minute version. A short practice still counts. There are no streaks.
- **AI feedback on each answer.** A feedback agent (Google Gemini, called through a small Cloudflare Worker) replies straight away with one thing done well, up to two corrections, and a suggested version. If the AI is unavailable, the page falls back to simple rules and says so.
- **Teacher dashboard.** Who is active, who has gone quiet (with a drafted personal message), course progress, a student detail page, and a weekly report: what to **focus on** in the live lesson and what to **mention briefly**, with evidence taken from the students' own answers.
- **Assistants.** Task Sender, Feedback Giver and Summariser, which the teacher can switch on or off.

## Run it locally
There is nothing to install. Download `index.html` from this repository and open it in any browser. Everything except the AI feedback runs inside that one page; the AI feedback calls the Worker in `worker/worker.js`, and the page falls back to simple built-in rules if it cannot reach it.

## How to try it (2 minutes)
1. Open the live demo and choose **I'm a teacher**. Read the dashboard and the **Weekly report**.
2. Press **Try the student side**, do a check-in, write an answer and read the AI feedback.
3. Go back to the teacher side: your check-in appears as a live row, and the report includes your answer.
4. Use the **demo day** arrows on the right to move through the week.

## What is real and what is simulated
- **Real:** the AI feedback on a student's answer, the dashboard and the weekly report, both computed from the answers in the page.
- **Simulated:** the sample class (six generated students), their answers and time spent, the assistants' daily run (simple rules), and message sending. Nothing is sent to anyone.
- **Not built yet:** accounts and logins, many teachers and classes, WhatsApp sending, voice and video tasks, the library and chats (shown as locked).

## Tech stack
- HTML, CSS and vanilla JavaScript in one page, hosted on GitHub Pages
- Cloudflare Workers (keeps the API key secret and returns feedback)
- Google Gemini API (feedback)
- Browser localStorage (demo state)
- Built with Claude (AI coding assistant)

## Files
- `index.html`: the whole app
- `worker/worker.js`: the feedback agent that runs on Cloudflare Workers (the API key is stored as a secret, not in this repository)

## Who built what
- **Kheira Kouider Aissa**: idea, teaching design (course structure, week rhythm, feedback criteria, report logic), testing, and all product decisions.
- **Claude (AI assistant)**: wrote the code and the Worker under her direction, and helped draft the texts.

## What happens next
Real accounts and classes for many teachers, WhatsApp sending, voice and video tasks, a library by interest, a course builder, and a pilot with my own learners to measure the time saved.
