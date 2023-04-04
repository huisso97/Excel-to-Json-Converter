# ExcelToJson Converter

하단의 이미지와 같이 다중 depths(이미지의 경우 4depths)의 tree 관계에 있는 테이블 데이터들을 파싱하여 HashMap 구조의 json으로 변환하는 converter입니다.
![excelImg](./image/excelImg.png)

기존 작업 시트 기반으로 수동으로 json으로 변환하는 것에 대한 비효율성과 시중에 있는 컨버터들로는 다중 depths의 테이블들 구조 파싱 처리가 되지 않아 만들게 되었습니다.

## 작업 환경 및 스택

Google AppsScript, JavaScript

## 진행 기간

2023.04.04 - 진행중

## 진행 단계

1. `SpreadsheetApp.getActiveSpreadsheet().getSheetByName('sheet명')`으로 sheet 불러오기
2. `sheet.getDataRange().getValues()`로 데이터 추출
3. 나누고자하는 depth를 상수 DEPTH로 지정

```javascript
const DEPTH = 4;
```

4. 각 테이블 및 행 단위로 데이터 구분 작업 진행 (빈 열이 테이블을 구분하는 기준)
5. link, linkName과 같이 여러 데이터가 하나의 행 데이터에 포함되는 경우에는 해당 데이터의 link, linkName과 합치는 작업 진행 ("/" 구분자 이용)
6. 앞의 "/"구분자로 합쳐진 link 값들은 array of object로 변환 작업 및 각 link object에 unique id 부여 (link 구분하기 위함)
7. uniqueid를 인덱스로 가지는 hashmap 구조로 변환

## 남은 작업

- [ ] link의 갯수가 유효할 때까지 계속 merge되도록 리팩토링 (기존에는 2개까지만 merge가 됐었음)
- [ ] depth 간 부모-자식 관계 처리를 위한 selections 프로퍼티 추가 및 자식 id 처리
