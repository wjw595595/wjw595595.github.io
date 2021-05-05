conn sys/123456@oracle01 as sysdba
column dest_name format a30
column destination format a20
column MEMBER format a45
column destination format a20
column TABLESPACE_NAME format a10
column FREE_RATE format a10
alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss';
set wrap off;
prompt  ****************************  实   例   状   态 ************************************;
select instance_name,version,status,database_status from v$instance;
prompt  ****************************  数 据 库 状 态 *************************************;
select name,log_mode,open_mode from v$database;
prompt  ****************************  控 制 文 件 状 态 ***********************************;
column name format a50
select status,name from v$controlfile;
prompt  ****************************  日 志 文 件 状 态 ***********************************;
select GROUP#,status,type,member from v$logfile;
prompt*****************************  归 档 目 的 地 状 态 *********************************;
select dest_name ,status,database_mode,destination from v$archive_dest_status where
dest_id in    ('1','2');
set heading off;
prompt    ************ 数 据 库 已 连 续 运 行 天 数*******************************************
select  round(a.atime-b.startup_time)||'  days  '  from(select  sysdate  atime  from  dual)
a,v$instance b;
set heading on;
prompt*****************************  会   话   数 *************************************;
select sessions_current,sessions_highwater from v$license;
prompt****************************  active  sessions  count **************************;
select count(*) "Active session count" from v$session where status='ACTIVE';
prompt****************************  total  sessions  count **************************•;
select count(*) "Total session count" from v$session;
prompt****************************  top  30  big  objects  name ************************;
column OWNER format a10
column SEGMENT_NAME format a35
column SEGMENT_TYPE format a15
column SIZES format a10
SELECT * FROM
 (
 select  OWNER,  SEGMENT_NAME,  SEGMENT_TYPE,  round(BYTES  /  1024  /
1024 / 1024,3)||'G' AS SIZES
   from dba_segments
   ORDER BY BYTES DESC)
     WHERE ROWNUM<=30
 ;
prompt*****************************  WANGGOUuser  data  size ****************************;
select  sum(bytes)/1024/1024/1024||'G'  "User  Data  Size"    from  dba_segments  where
owner='WANGOU';

prompt*****************************  SUP  data  size ************************************;
select  sum(bytes)/1024/1024/1024||'G'  "User  Data  Size"    from  dba_segments  where
owner='SUP';

prompt*****************************  DB  size ******************************************;
select sum(bytes)/1024/1024/1024||'G' "DB Size"    from dba_segments;

prompt*****************************  total  tablespace  size ****************************;
select  sum(bytes)/1024/1024/1024||'G'  "Total  Tablespace    Size"    from
dba_data_files;

prompt*****************************  last  day  archive  log  count ***********************;
select  sequence#,  completion_time  from  v$archived_log  where  completion_time>=
trunc(sysdate-1) and completion_time<= trunc(sysdate) and dest_id=1;

prompt********************** 表空间监控********************;
SELECT D.TABLESPACE_NAME,
         SPACE "SUM_SPACE(M)",
         BLOCKS "SUM_BLOCKS(K)",
         SPACE - NVL (FREE_SPACE, 0) "USED_SPACE(M)",
         ROUND(  (1  -  NVL  (FREE_SPACE,  0)  /  SPACE)  *  100,  2)
"USED_RATE(%)",
         FREE_SPACE "FREE_SPACE(M)"
  FROM      (    SELECT      TABLESPACE_NAME,
                     ROUND (SUM (BYTES) / (1024 * 1024), 2) SPACE,
                     SUM (BLOCKS) BLOCKS
              FROM      DBA_DATA_FILES
          GROUP BY      TABLESPACE_NAME) D,
         (    SELECT      TABLESPACE_NAME,
                     ROUND  (SUM  (BYTES)  /  (1024  *  1024),  2)
FREE_SPACE
              FROM      DBA_FREE_SPACE
                        GROUP BY      TABLESPACE_NAME) F
 WHERE      D.TABLESPACE_NAME = F.TABLESPACE_NAME(+)
UNION ALL                                                                                                     --如果有
SELECT      D.TABLESPACE_NAME,
         SPACE "SUM_SPACE(M)",
         BLOCKS SUM_BLOCKS,
         USED_SPACE "USED_SPACE(M)",
         ROUND  (NVL  (USED_SPACE,  0)  /  SPACE  *  100,  2)
"USED_RATE(%)",
         NVL (FREE_SPACE, 0) "FREE_SPACE(M)"
  FROM      (    SELECT      TABLESPACE_NAME,
                     ROUND (SUM (BYTES) / (1024 * 1024), 2) SPACE,
                     SUM (BLOCKS) BLOCKS
              FROM      DBA_TEMP_FILES
          GROUP BY      TABLESPACE_NAME) D,
         (    SELECT      TABLESPACE_NAME,
                     ROUND  (SUM  (BYTES_USED)  /  (1024  *  1024),  2)
USED_SPACE,
                     ROUND  (SUM  (BYTES_FREE)  /  (1024  *  1024),  2)
FREE_SPACE
              FROM      V$TEMP_SPACE_HEADER
              GROUP BY      TABLESPACE_NAME) F
 WHERE      D.TABLESPACE_NAME = F.TABLESPACE_NAME(+)
ORDER BY      1;
prompt  ****************************  表 空 间OFFLINE(显 示 为 空 正 常) ********************;
select tablespace_name ,status    from dba_tablespaces where status='OFFLINE';
prompt  ****************************  SEQUENCE 同步数 *********************************;
select max(sequence#)from v$log_history;
conn sys/123456@oracle01 as sysdba
prompt  ****************************  备库SEQUENCE 同步数 *****************************;
select max(sequence#)from v$log_history;
prompt  ****************************  备库日志未应用(显 示 为 空 正 常) *******************;
select sequence#,applied from v$archived_log where applied='yes';
prompt  **************************** 备库日志应用(显示最近十个日志) *****************;
select * from(select sequence#,applied from v$archived_log order by sequence# desc)
where rownum<=10;
set time on
disconnect
exit

参考：
https://www.cnblogs.com/kingle-study/p/10997563.html

oracle_exporter
https://github.com/iamseth/oracledb_exporter#installation