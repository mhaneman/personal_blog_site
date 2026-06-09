---
title: Wordle with Friends
description: "A fullstack daily Wordle clone with friends, leaderboards, and a Go/React stack."
tags: ["webdev", "Go", "React", "PostgreSQL", "Docker"]
categories: ["Web Development"]
---

[GitHub](https://github.com/mhaneman/wordle-with-friends) · [thesecretword.xyz](https://thesecretword.xyz)

My family started sharing daily Wordle scores over text, so I built a small fullstack app for us — usually around 3–5 daily players. The backend is a **Go/chi** REST API with **PostgreSQL** leaderboards; the frontend is **React**, **Vite**, and **Tailwind**.

It uses JWT cookie auth, server-authoritative daily puzzles, and invite-only registration. The whole thing runs on a VPS with **Docker Compose**, **Caddy** for HTTPS, and automated database migrations.

Demo credentials are available on request.
