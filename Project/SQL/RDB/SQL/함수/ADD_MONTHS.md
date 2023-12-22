## 🌈 ADD_MONTHS

- 날짜에 숫자만큼 개월 수를 더하여 반환
- 이전 달이나 다음 달에 해당 날짜가 존재하지 않으면 해당 달의 마지막 일자 반환

```sql
SELECT ADD_MONTHS(TO_DATE('2023-12-31', 'YYYY-MM-DD'), -1) FROM DUAL; // 2023-11-30
```