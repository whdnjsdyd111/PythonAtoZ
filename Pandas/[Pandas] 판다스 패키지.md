[Pandas] 판다스
==============

Pandas 는 스프레드시트나 데이터베이스처럼 관계형 또는 **레이블** 형태의 표 형식의 데이터를 쉽고 직관적으로 작업하는데 적합한 도구다.
이 뿐만 아니라 데이터 탐색, 분석, 정리 및 처리에 큰 도움이 된다.
기본적인 두 가지 데이터 구조인 `Series` (1차원) 및 `DataFrame` (2차원)은 금융, 통계, 사화 과학, 엔지니어링 등 일반적인 사례들을 처리한다.

- 특징으로는 부동 · 비부동 소수점 데이터에서 **누락된 데이터(`NaN`)을 쉽게 처리**
- DataFrame 및 더 높은 차원을 쉽게 **삽입/삭제**
- 자동 **데이터 정렬**
- 강력하고 유연한 기능별 **그룹화**
- 다른 데이터 구조(Numpy 등)을 DataFrame 객체로 쉽게 **변환**
- 필요한 데이터를 쉽게 **필터링**

![01_table_dataframe](https://user-images.githubusercontent.com/66655578/167612016-d09366ef-dba2-437f-9760-9e614c138e9f.svg)

표 형식의 데이터를 다루기 때문에 즉시 사용 가능한 다양한 파일의 형식이나 데이터 소스(csv, Excel, sql, json, parquet 등)와 통합을 지원한다.
이러한 데이터 소스를 각각 가져오고 내보내는데 `read_*`, `to_*` 메소드로 쉽게 사용한다.

![02_io_readwrite](https://user-images.githubusercontent.com/66655578/167613027-33b89b71-5671-4a06-bd64-298b9b3873a8.svg)

Matplotlib 라이브러리 기능을 사용하여 데이터 플로팅을 쉽게 할 수 있다.

![04_plot_overview](https://user-images.githubusercontent.com/66655578/167613287-907eea06-bde8-4409-a659-861510fc0d0c.svg)
