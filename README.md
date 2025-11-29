# Power-of-Friendship_SportsEcon
# MLB Playoff Chemistry Analysis

This repository contains the code and data pipeline used to construct my novel proxy for team chemistry, **The Power of Friendship** in MLB playoff games  to estimate its effect on game outcomes. The project compiles extensive information from **332 postseason games** played between 2014 and 2024 (excluding 2020), and introduces a chemistry metric based on interview transcripts from players and coaches.

**READ**
#This was a major project done within a few days, and also my first time using Google Colab---my focus was obtaining the dataset quickly as opposed to creating a clean replicable program; thus the formatting and organization of this project is not very clean and/or optimized. I include it rather as a general overview of the process that went into the data collections for my Sports Econ paper.
---

## 📌 Overview

The central contribution of this project is the creation of a unique, behavior-based measure of team chemistry. Rather than relying on peer effects, shared characteristics, or past-game momentum, this work uses direct language from team personnel to quantify how frequently teammates mention one another in game-day interviews.

This measure, **Teammate Mention Frequency (TMF)**, captures real-time social cohesion using text scraped from official transcript sources.

---

## 📂 Data Collection and Sample Construction

### **Interview Transcript Scraping**
- Pre-game and post-game interview transcripts were scraped from **ASAP Sports** using Python’s `Requests` and `BeautifulSoup` packages.
- The sample includes **3684 transcripts** corresponding to the 332 games studied.
- Transcripts include players, coaches, intermediaries, MLB executives, and umpires.

### **Team Identification**
Because ASAP Sports transcripts do not indicate team affiliation:
- Each interviewee was matched to their team using a **Wikipedia-based helper function** built with `Requests` and JSON parsing.
- **3599 transcripts** were successfully matched to a team.
- **85 transcripts** corresponding to executives, umpires, or neutral personnel were removed.

### **Filtering for Game-Day Chemistry**
To measure chemistry specific to each game:
- Only **game-day interviews** were retained.
- **2784 interview transcripts** occurred on game days.
- **815 pre-series or travel-day interviews** were excluded.


## Constructing the Team Chemistry Variable (TMF)


### **Roster Reference List**
- Full **26-man playoff rosters** were obtained for every team-year using the **MLB StatsAPI**.
- Each transcript was checked word-by-word against its team’s roster.

### **Counting Mentions**
A teammate mention was counted when:
- A rostered player’s **first name**, **last name**, **first-last**, or **last-first** appeared.
- The `TheFuzz` string-similarity library detected close matches (capturing nicknames and abbreviations).

### **Aggregation**
For each team in each game:
- Total teammate mentions were summed across all interview transcripts.
- Total spoken words were also summed.
- TMF was calculated as:


### **TMF Summary**
- **Average TMF:** 0.61% of words (≈ 1 mention per 200 words)
- **Range:** 0% to 2.43%
- **Highest:** Kansas City Royals on Oct 14, 2015 (39 mentions in 1608 words)
- **Lowest:** Cleveland Guardians on Oct 8, 2018 (0 mentions in 1020 words)
- Winning teams exhibited **higher average TMF** than losing teams:  
  - Winners: **0.64%**  
  - Losers: **0.58%**

---

## 📊 Additional Variables Collected

### **Team-Level Controls**  
From the `pybaseball` package, the following season-level metrics were obtained:

- **OPS+**, **ERA+**, **FIP**
- **Runs Scored (RS)** and **Runs Allowed (RA)**
- **oWAR**, **dWAR**, **pWAR**
- **Regular Season Win Percentage (WP)**
- **September Win Percentage (SWP)** (proxy for late-season momentum)

These statistics approximate true team quality over a 162-game season.

### **Game-Level Controls**
Binary variables were created for:
- `is_Division`  
- `is_Championship`  
- `is_World`

These indicators capture differences across playoff rounds (stakes, fatigue, etc.).

A separate variable measures **home-field advantage (HFA)**, which has documented impacts in comparable sports settings (e.g., NHL per Leard 2011).

---

## 🎯 Dependent Variable

The dependent variable used in all models is:

- **TW (Team Won)**  
  - `1` if the team won the game  
  - `0` otherwise  

Descriptive statistics are provided in the paper’s Appendix Table 1.




