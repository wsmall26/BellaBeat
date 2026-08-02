-- =====================================================================
-- Combine Fitbit-style Activity + Sleep data by Member_ID
-- Source files:
--   1) combined_users_1.xlsx  -> sheet "Combined by User"  (35 members)
--   2) sleepDay_combined_by_member.csv                     (24 members)
-- =====================================================================

-- ---------------------------------------------------------------------
-- 1. STANDARDIZED TABLE DEFINITIONS
--    Column names normalized to snake_case, consistent types:
--    member_id  -> BIGINT (10-digit device ID, same format in both sources)
--    dates      -> DATE (ISO 8601, YYYY-MM-DD)
--    all numeric aggregates -> NUMERIC / INTEGER as appropriate
-- ---------------------------------------------------------------------

CREATE TABLE activity_by_member (
    member_id                   BIGINT PRIMARY KEY,
    days_tracked                INTEGER,
    first_date                  DATE,
    last_date                   DATE,
    total_steps_sum             INTEGER,
    total_steps_avg             NUMERIC,
    total_distance_sum          NUMERIC,
    total_distance_avg          NUMERIC,
    very_active_minutes_avg     NUMERIC,
    fairly_active_minutes_avg   NUMERIC,
    lightly_active_minutes_avg  NUMERIC,
    sedentary_minutes_avg       NUMERIC,
    calories_sum                INTEGER,
    calories_avg                NUMERIC,
    zero_step_days              INTEGER
);

CREATE TABLE sleep_by_member (
    member_id                    BIGINT PRIMARY KEY,
    days_logged                  INTEGER,
    total_sleep_sessions         INTEGER,
    total_minutes_asleep         INTEGER,
    total_time_in_bed            INTEGER,
    avg_minutes_asleep_per_day    NUMERIC,
    avg_time_in_bed_per_day       NUMERIC
);

-- ---------------------------------------------------------------------
-- 2. NULL / DATA-QUALITY CHECKS (run before combining)
-- ---------------------------------------------------------------------

SELECT 'activity_by_member' AS table_name, COUNT(*) AS total_rows,
       SUM(CASE WHEN member_id IS NULL THEN 1 ELSE 0 END) AS null_member_id,
       SUM(CASE WHEN total_steps_sum IS NULL THEN 1 ELSE 0 END) AS null_steps,
       SUM(CASE WHEN calories_sum IS NULL THEN 1 ELSE 0 END) AS null_calories
FROM activity_by_member;

SELECT 'sleep_by_member' AS table_name, COUNT(*) AS total_rows,
       SUM(CASE WHEN member_id IS NULL THEN 1 ELSE 0 END) AS null_member_id,
       SUM(CASE WHEN total_minutes_asleep IS NULL THEN 1 ELSE 0 END) AS null_minutes_asleep
FROM sleep_by_member;

-- Result: 0 nulls found in either source table.

-- ---------------------------------------------------------------------
-- 3. COMBINE ON MATCHING member_id (LEFT JOIN keeps all 35 activity
--    members; sleep columns are NULL for the 11 members who have no
--    matching sleep log)
-- ---------------------------------------------------------------------

SELECT
    a.member_id,
    a.days_tracked,
    a.first_date,
    a.last_date,
    a.total_steps_sum,
    a.total_steps_avg,
    a.total_distance_sum,
    a.total_distance_avg,
    a.very_active_minutes_avg,
    a.fairly_active_minutes_avg,
    a.lightly_active_minutes_avg,
    a.sedentary_minutes_avg,
    a.calories_sum,
    a.calories_avg,
    a.zero_step_days,
    s.days_logged                 AS sleep_days_logged,
    s.total_sleep_sessions,
    s.total_minutes_asleep,
    s.total_time_in_bed,
    s.avg_minutes_asleep_per_day,
    s.avg_time_in_bed_per_day
FROM activity_by_member a
LEFT JOIN sleep_by_member s
    ON a.member_id = s.member_id
ORDER BY a.member_id;

-- ---------------------------------------------------------------------
-- 4. NULLS INTRODUCED BY THE JOIN (members with no sleep log)
-- ---------------------------------------------------------------------

SELECT a.member_id, a.days_tracked
FROM activity_by_member a
LEFT JOIN sleep_by_member s ON a.member_id = s.member_id
WHERE s.member_id IS NULL
ORDER BY a.member_id;

-- Result: 11 member_ids with activity data but no sleep log:
-- 1624580081, 2022484408, 2873212765, 2891001357, 3372868164,
-- 4057192912, 6290855005, 6391747486, 8253242879, 8583815059, 8877689391

-- ---------------------------------------------------------------------
-- 5. OPTIONAL: INNER JOIN if you only want members present in BOTH
--    datasets (24 rows, no nulls from the join)
-- ---------------------------------------------------------------------

-- SELECT a.*, s.*
-- FROM activity_by_member a
-- INNER JOIN sleep_by_member s ON a.member_id = s.member_id
-- ORDER BY a.member_id;
