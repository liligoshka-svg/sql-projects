WITH
  message_date AS (
    SELECT
      DATE_ADD(s.date, INTERVAL es.sent_date day) AS sent_date,
      acs.account_id AS id_account, id_message AS id_message
    FROM `DA.email_sent` es
    JOIN `DA.account_session` acs
      ON es.id_account = acs.account_id
    JOIN `DA.session` s
      ON acs.ga_session_id = s.ga_session_id
  ),
  count_msg AS (
    SELECT
      DATE_TRUNC(message_date.sent_date, month) AS sent_month,
      message_date.id_account,
      COUNT(distinct message_date.id_message) AS mess_month_cnt,
      MIN(message_date.sent_date) AS first_sent_date,
      MAX(message_date.sent_date) AS last_sent_date
    FROM message_date
    GROUP BY 1, 2
  ),
  cnt_all AS (
    SELECT
      count_msg.sent_month AS sent_month,
      SUM(count_msg.mess_month_cnt) AS all_msg
    FROM count_msg
    GROUP BY 1
  )
SELECT
  count_msg.sent_month,
  count_msg.id_account,
  count_msg.mess_month_cnt / cnt_all.all_msg * 100
    AS sent_msg_percent_from_this_month,
  count_msg.first_sent_date,
  count_msg.last_sent_date
FROM count_msg
JOIN cnt_all
  ON count_msg.sent_month = cnt_all.sent_month
