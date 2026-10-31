# Omniloop

**An AI system that lifts teachers' daily load: it sends tasks, gives feedback and turns students' input into better lessons.**

**Live demo (no install, no sign-in):** https://khirakaisaa-bit.github.io/OmniLoop/

## The problem
Learners keep asking their teacher for something to practise between live sessions, because progress comes from daily immersion, not one lesson a week. But sending tasks, chasing replies, correcting answers and spotting who is struggling by hand is too much for one teacher, so daily practice fades and the live lesson is planned without knowing what learners found difficult.

## What Omniloop does
- **Course-based daily tasks.** Each course week has an objective. It is split into four small daily tasks, plus a cheat day (catch up), a rest day, and the live session.
- **Mood-based length.** Students check in with how they feel and choose a 2-minute or a 10-minute version. A short practice still counts. There are no streaks.
- **AI feedback on each answer.** A feedback agent (Google Gemini, called through a small Cloudflare Worker) replies straight away with one thing done well, up to two corrections, and a suggested version. If the AI is unavailable, the page falls back to simple rules and says so.
- **Voice answers and Listen.** Students can speak an answer instead of typing (Chrome or Edge) and press Listen to hear a task or their improved version read aloud. Both use the browser's own speech features.
- **Teacher dashboard.** Who is active, who has gone quiet (with a drafted personal message), course progress, a student detail page, and a weekly report: what to **focus on** in the live lesson and what to **mention briefly**, with evidence taken from the students' own answers.
- **AI lesson brief (Summariser agent).** From the week's answers, a second AI agent writes a short brief for the live lesson: what to focus on, what to mention briefly, an outline and who needs a personal word. Every quote it shows is checked against the student's real answer, and anything not found word for word is dropped. The teacher can copy it or download it as a text file.
- **Add a student.** The teacher adds a student by name and gets an invite code and link. This is a preview of invite-only joining: no real account is created.
- **Assistants page.** The Feedback Giver and the Summariser are live AI agents, and the teacher can switch either off. The Task Sender agent is shown as coming soon.

## Run it locally
There is nothing to install. Download `index.html` from this repository and open it in any browser. Everything except the AI feedback runs inside that one page; the two AI agents call the Worker in `worker/worker.js`, and the page falls back to simple built-in rules if it cannot reach it.

## How to try it (2 minutes)
1. Open the live demo and choose **I'm a teacher**. Read the dashboard, the **Course** tab and the **Weekly report**.
2. Press **Try the student side**, do a check-in, write an answer and read the AI feedback. Revise your answer once.
3. Go back to the teacher side: your check-in appears as a live row, and the report includes your answer.
4. Use the **demo day** arrows on the right to move through the week.
5. On the **Weekly report**, press **Write the AI brief** and then **Download as a text file**.
6. On **Students**, add a student to see an invite code.
7. Open **Assistants** and switch the Feedback Giver off to see the student get simple built-in feedback instead.

## How it works
```
Student's browser (index.html on GitHub Pages)
   |  answer text + task number, or the week's answers   (HTTPS, only from the allowed site)
   v
Cloudflare Worker (worker/worker.js)   <- API key stored as a secret here, never in this repository
   |  instructions + the answer
   v
Google Gemini API  ->  structured feedback or brief (JSON)  ->  back to the page
```
- The page saves answers in the browser (localStorage). The teacher's dashboard and weekly report are computed from those saved answers.
- If the Worker or Gemini does not answer, the page retries once and then falls back to simple built-in rules, and labels the feedback as simulated.
- For the brief, the Worker keeps only quotes found word for word in that student's answer, and the page checks them again.
- The Worker only accepts requests from the demo site, limits how often it can be called, and treats the student's text as data, not as instructions.

## What is real and what is simulated
- **Real:** the AI feedback on a student's answer, the AI lesson brief, voice answers and Listen (browser speech), the text-file download, the dashboard and the weekly report, computed from the answers in the page.
- **Simulated:** the sample class (six generated students), their answers and time spent, and the drafted messages (nothing is sent to anyone). The weekly report numbers are computed with simple rules; only the lesson brief is written by AI. Invite codes are a preview and create no account.
- **Not built yet:** accounts and logins, teacher verification and invite-only student sign-up, many teachers and classes, automatic reminders, the Task Sender agent, video tasks, subscriptions, the library and chats (shown as locked or coming soon).

## Tech stack
- HTML, CSS and vanilla JavaScript in one page, hosted on GitHub Pages
- Cloudflare Workers (keeps the API key secret and returns feedback)
- Google Gemini API (feedback and Summariser agents)
- Browser Web Speech API (voice answers, Listen)
- Browser localStorage (demo state)
- Built with Claude (AI coding assistant)

## Files
- `index.html`: the whole app
- `worker/worker.js`: the two agents (feedback and brief) that run on Cloudflare Workers (the API key is stored as a secret, not in this repository)
- `worker/test.mjs`: a small test for the Worker (run with `node worker/test.mjs`; it uses a pretend Gemini, so no key is needed)

## Who built what
- **Kheira Kouider Aissa**: idea, teaching design (course structure, week rhythm, feedback criteria, report logic), testing, and all product decisions.
- **Claude (AI assistant)**: wrote the code and the Worker under her direction, and helped draft the texts.

## What happens next
A pilot with my own learners to measure the time saved; real accounts with teacher verification and invite-only student sign-up; automatic reminders by email and Telegram; the Task Sender agent; video tasks; teachers importing their own courses; a library of extra practice; teacher and student subscriptions.
