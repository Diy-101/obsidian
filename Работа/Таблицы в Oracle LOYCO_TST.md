```sql
select * from loyco.LM_EXTERNAL_ENTITY_LINKS where ID_EXTERNAL_ENTITY='5477314521'; --основная таблица с сущностями
select * from loyco.LM_EXTERNAL_ENTITY_TYPES; --типы сущностей
select * from loyco.LM_ENTITIES where ID_ENTITY=265304229; --связки договоров и программ лояльности

select * from loyco.lm_programs where ID_PROGRAM=33;--программы лояльности
select * from loyco.LIFESTYLE_OPERATION_RULES where PROGRAM_ID=33 and END_DATE is null;
select * from loyco.LM_CONSTANT_TYPES;--типы констант
select * from loyco.LM_PROGRAM_CONSTANTS where PROGRAM_ID=33 and CONSTANT_TYPE_ID=59;--программные константы
select * from loyco.LM_PROGRAM_CONSTANT_VALUES where CONSTANT_ID in (12399,12397,12398,12404);--значения констант
select * from loyco.BUNDLE_types;--типы подписок
select * from loyco.LM_ACCOUNT_TYPES;--типы счетов

--проверка балансов LM
select distinct la.id_entity , la.id_type, lat.CODE_TYPE, la.id_program, la.balance, la.total_debit, la.total_credit, la.stmt_debit, la.stmt_credit, lel.id_external_entity--,trim(a.numberx) numberx,  lp.name--,a.stgeneral
from loyco.LM_ACCOUNTS la
join loyco.lm_external_entity_links lel
on la.id_entity= lel.id_entity
join loyco.lm_programs lp
on lp.id_program= la.id_program
join loyco.LM_ACCOUNT_TYPES lat on lat.ID_TYPE=la.ID_TYPE
where 1=1
and  lel.id_external_entity in ('5477314521'--номер договора
)
order by  lel.id_external_entity,la.id_program ,la.id_type;

--проверка счетчиков
select c.*,o.* from loyco.lm_counters c
join loyco.lm_counter_types o on o.counter_type_id=c.counter_type_id
join loyco.lm_external_entity_links e on e.id_entity=c.entity_id
join loyco.lm_entity_statements st on st.statement_id=c.statement_id and st.entity_id=c.entity_id
where e.id_entity in (265304229);
select * from loyco.LM_ENTITY_STATEMENTS where ENTITY_ID=265304229; --выписки договоров

select * from loyco.COUNTER_RECALCULATE_QUEUE;--таблица для пересчета глобалов
select * from loyco.PRIME_OPERATION_GLOBAL_RULES where PROGRAM_ID=33;--глобалы
```