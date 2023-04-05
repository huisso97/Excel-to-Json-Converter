# ExcelToJson Converter

하단의 이미지와 같이 다중 depths(이미지의 경우 4depths)의 tree 관계에 있는 테이블 데이터들을 파싱하여 HashMap 구조의 json으로 변환하는 converter입니다.
![excelImg](./image/excelImg.png)

기존 작업 시트 기반으로 수동으로 json으로 변환하는 것에 대한 비효율성과 시중에 있는 컨버터들로는 다중 depths의 테이블들 구조 파싱 처리가 되지 않아 만들게 되었습니다.

## 작업 환경 및 스택

Google AppsScript, JavaScript

## 진행 기간

2023.04.04 - 진행중

## 사용 방법

1. 첨부한 구글 시트 이미지와 같이, 구글 시트를 테이블을 기준으로 depths를 나누고, 수평으로 부모-자식 관계를 설정한다.

2. link와 linkname은 복수의 갯수를 설정할 수 있다.
3. AppsScript에 converter 함수를 넣고, sheet 불러오는 코드에 대한 주석을 해제한다.
4. 하단의 headers와 rows 상수값들은 주석처리한다.
5. 함수 실행

## 진행 단계

1. `SpreadsheetApp.getActiveSpreadsheet().getSheetByName('sheet명')`으로 sheet 불러오기
2. `sheet.getDataRange().getValues()`로 데이터 추출
3. 시트마다 달라질 변수들 상수화하여 처리

```javascript
const PROPERTY_LENGTH = 8;
```

4. 각 테이블 및 행 단위로 데이터 구분 작업 진행 (빈 열이 테이블을 구분하는 기준)
5. 이때 link와 linkName과 같이 옵션들이 있는 프로퍼티를 마주칠 경우, 해당 행을 부모로 잡고, 아래 행들을 순회하며 옵션값들이 있으면 누적
6. 프로퍼티들을 다 채웠고, 최종 데이터인 것에 한해서(ex. title에 값이 있는 경우) result에 추가
7. link 옵션들 관심사 분리 작업 :
   - link와 linkName들을 하나의 dataset으로 만들어 links라는 property에 array of object로 추가 및 각 link object에 unique id 부여 (link 구분하기 위함)
8. uniqueid를 인덱스로 가지는 hashmap 구조로 변환
9. 이중 반복문으로 자신의 title을 upperTitle로 가지고 있는 자식 데이터의 uniqueid를 selection에 추가하여 부모 자식 관계 설정.
   - 꼭 upperTitle이 아니더라도 uinque한 값을 기준으로 연결 지으면 됨

### upperTitle 프로퍼티 설정 이유

converter가 부모와 자식관계를 파악하기 위해 upperTitle 등과 같은 unique 값이 있어야한다.

- 이렇게 프로퍼티를 추가한 이유는 엑셀 내 공백의 예외 사항이 link의 옵션일 경우, 앞의 상위 뎁스에 대한 공백일 경우, 하위 뎁스에 대한 공백일 경우, 예상치 못하여 저장된 공백일 경우 등, 다양한 예외 케이스가 발생하기 때문임

## 남은 작업

- [ x ] link의 갯수가 유효할 때까지 계속 merge되도록 리팩토링 (기존에는 2개까지만 merge가 됐었음)
- [ x ] depth 간 부모-자식 관계 처리를 위한 selections 프로퍼티 추가 및 자식 id 처리
