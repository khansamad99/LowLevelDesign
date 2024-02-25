# Cricbuzz Low-Level Design (LLD) Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [Requirement Gathering](#requirement-gathering)
3. [Flow](#flow)
4. [Objects Identification (Classes)](#objects-identification-classes)
5. [Classes Description](#classes-description)
6. [Concurrency Management](#concurrency-management)
7. [Observer Design Pattern](#observer-design-pattern)

## Introduction

This document outlines the low-level design for the Cricbuzz application, focusing on real-time cricket match updates, including ball-by-ball action, player statistics, and scorecard updates. The design covers the core functionalities required to simulate a cricket match's scoring and update mechanism on the Cricbuzz platform.

## Requirement Gathering

Cricbuzz aims to provide live updates of cricket matches, including detailed scorecards that get updated after every ball. Key requirements include:

- Display multiple ongoing matches
- Update scorecards ball-by-ball
- Maintain detailed records of each player's performance (batting and bowling)
- Handle innings switch and calculate match outcomes

## Flow

1. Match setup with two teams
2. Innings begin, detailing over-by-over play
3. Players' performance updated in real-time
4. Match completion and winner declaration

## Objects Identification (Classes)

- Match
- Team
- Player
- Innings
- Over
- Ball
- Scorecard (Batting and Bowling)
- Observer for score updates

## Classes Description

### Match

- String `matchId`
- Team `teamA`
- Team `teamB`
- Date `matchDate`
- String `venue`
- List<Innings> `innings`
- MatchType `matchType`
- Team `tossWinner`

### Team

- String `teamId`
- String `name`
- List<Player> `players`
- PlayerBattingController `battingController`
- PlayerBowlingController `bowlingController`

### Player

- String `playerId`
- String `name`
- int `age`
- String `role` (Batsman, Bowler, Wicketkeeper, All-rounder)
- BattingScorecard `battingScorecard`
- BowlingScorecard `bowlingScorecard`

### Innings

- int `inningsNumber`
- Team `battingTeam`
- Team `bowlingTeam`
- List<Over> `overs`
- startInnings()

### Over

- int `overNumber`
- List<Ball> `balls`
- startOver()

### Ball

- int `ballNumber`
- String `type` (Normal, Wide, No Ball) ENUM
- RunType `runType` ENUM
- Player `playedBy`
- Player `bowledBy`

### BattingScorecard

- int `runs`
- int `ballsFaced`
- int `fours`
- int `sixes`
- double `strikeRate`

### BowlingScorecard

- int `overs`
- int `maidens`
- int `runsGiven`
- int `wickets`
- int `noBalls`
- int `wideBalls`
- double `economyRate`

## Concurrency Management

Implement a mechanism to handle real-time updates to the scorecard, ensuring data consistency across multiple simultaneous accesses.

## Observer Design Pattern

Utilize the Observer design pattern to update batting and bowling scorecards after each ball. This includes:

### ScoreUpdaterObserver

An interface to update scorecards.

#### Implementations:

- **BattingScorecardUpdater** -> update(Ball)
- **BowlingScorecardUpdater** -> update(Ball)

These observers are notified after every ball to update player statistics accordingly, ensuring that the scorecards reflect the most current match state.

---

This LLD documentation outlines the foundational structure for the Cricbuzz app, focusing on managing cricket match data and ensuring live updates are accurately reflected. The design leverages object-oriented principles and the Observer design pattern to maintain data consistency and real-time updates.
