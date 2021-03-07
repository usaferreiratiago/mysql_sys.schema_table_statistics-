# mysql_sys.schema_table_statistics-

SELECT 
    `pst`.`OBJECT_SCHEMA` AS `table_schema`,
    `pst`.`OBJECT_NAME` AS `table_name`,
    FORMAT_PICO_TIME(`pst`.`SUM_TIMER_WAIT`) AS `total_latency`,
    `pst`.`COUNT_FETCH` AS `rows_fetched`,
    FORMAT_PICO_TIME(`pst`.`SUM_TIMER_FETCH`) AS `fetch_latency`,
    `pst`.`COUNT_INSERT` AS `rows_inserted`,
    FORMAT_PICO_TIME(`pst`.`SUM_TIMER_INSERT`) AS `insert_latency`,
    `pst`.`COUNT_UPDATE` AS `rows_updated`,
    FORMAT_PICO_TIME(`pst`.`SUM_TIMER_UPDATE`) AS `update_latency`,
    `pst`.`COUNT_DELETE` AS `rows_deleted`,
    FORMAT_PICO_TIME(`pst`.`SUM_TIMER_DELETE`) AS `delete_latency`,
    `sys`.`fsbi`.`count_read` AS `io_read_requests`,
    FORMAT_BYTES(`sys`.`fsbi`.`sum_number_of_bytes_read`) AS `io_read`,
    FORMAT_PICO_TIME(`sys`.`fsbi`.`sum_timer_read`) AS `io_read_latency`,
    `sys`.`fsbi`.`count_write` AS `io_write_requests`,
    FORMAT_BYTES(`sys`.`fsbi`.`sum_number_of_bytes_write`) AS `io_write`,
    FORMAT_PICO_TIME(`sys`.`fsbi`.`sum_timer_write`) AS `io_write_latency`,
    `sys`.`fsbi`.`count_misc` AS `io_misc_requests`,
    FORMAT_PICO_TIME(`sys`.`fsbi`.`sum_timer_misc`) AS `io_misc_latency`
FROM
    (`performance_schema`.`table_io_waits_summary_by_table` `pst`
    LEFT JOIN `sys`.`x$ps_schema_table_statistics_io` `fsbi` ON (((`pst`.`OBJECT_SCHEMA` = `sys`.`fsbi`.`table_schema`)
        AND (`pst`.`OBJECT_NAME` = `sys`.`fsbi`.`table_name`))))
ORDER BY `pst`.`SUM_TIMER_WAIT` DESC
