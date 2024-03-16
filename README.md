설명가능한 인공지능을 활용한 대화형 UAM 관제 API
==============================================
* 한국항공우주학회 2024 춘계 학술대회
* 미래 UAM 산업에 대비하여 설명가능한 대화형 UAM (Urban Air Mobility) 교통 관제 시스템을 제안
* 지식 그래프를 통해 교통 상황에 대한 인지, 상황 변화에 대한 시스템의 설명 가능성을 확보하고 LLM 환각현상을 방지함
* Langchain을 활용하여 지식그래프를 지식기반으로 사용하는 LLM 시스템 구성

   


배경
-----
UAM은 도심 저고도 공역(고도 300m~600m)에서 고밀도로 운항될 예정이기 때문에 다양한 외부 요소의 영향을 받는다. 그 중에서도 날씨, 비협력적 비행체와 조우와 같은 예측 불가한 변수에 의한 교통 상황의 변화는 UAM 운항 스케줄의 전면적인 재조정을 빈번하게 요구할 것으로 예상되기 때문에 기존 항공 운항관제 시스템과 차별화된 교통 관제 시스템이 요구된다. 

따라서 지식그래프과 대화형 시스템을 결합하여 UAM 교통 관제의 효율성과 안전성을 향상시키는 것을 목적으로 할 뿐만 아니라, 제안한 설명가능한 인공지능 관제 시스템을 통해 UAM의 신뢰성을 높이고, 사용자와 사회에 긍정적인 영향을 미칠 것으로 기대하고 있으며, 최종적으로는 UAM 관제 시스템의 설계 및 운영에 있어 중요한 지침과 통찰을 제공하고자 한다.   


UAM 운행 스케줄 데이터 생성
--------------------------

지식그래프를 구축하기 위해 Time, UAM, Vertiport, Vertipad, Waypoint, Area 총 6개의 노드와 속성을 정의하고 이에 대한 CSV 데이터를 생성하였다.


| **UAM**                                   | **VERTIPORT**                                   | **VERTIPAD**                                    | **AREA**                          | **WAYPOINT** | **TIME** |
|-------------------------------------------|-------------------------------------------------|-------------------------------------------------|-----------------------------------|--------------|----------|
| uam_id: UAM 1 - UAM 8                     | vertiport_id: total 3                           | vertipad_id                                     | area_id                           | waypoint_id  | time     |
| waypoint_id: currently located waypoint   | vertipad_id: vertipads located in the vertiport | vertiport_id: vertiport containing the vertipad | availability: flight availability | area_id      |          |
| vertiport_id: currently located vertiport | area_id: area containing the vertiport          | uam_id: uam located in the vertipad             |                                   | uam_id       |          |
| vertipad_id: currently located vertipad   | time                                            | time                                            |                                   | time         |          |
| time                                      |                                                 |                                                 |                                   |              |          |    



UAM 교통관제 온톨로지 모델링
---------------------------
![image](https://github.com/dododadadada/Neo4j-GraphQA-for-UAM/assets/98035735/cf180e99-9b22-452b-8cdb-8bedfcdf804d)    



대화형 UAM 교통 관제 API 구축
-----------------------------
![image](https://github.com/dododadadada/Neo4j-GraphQA-for-UAM/assets/98035735/05f782b8-e56a-4956-b343-77543f26b344)    



실험 시나리오
---------------
앞서 설계한 시스템의 정확성과 타당성을 평가하기 위해 시스템 내에서 계획된 운행 범위를 준수하는 정상 시나리오와 시스템 내에 계획된 운행 범위를 벗어나는 비정상 시나리오로 구분했다. 

**정상 시나리오**

정상 시나리오는 계획된 uam 운항 스케줄에 따라 정상적으로 운행하고 있을 때 시스템과 운영자 간 정보 교환이 올바르게 이루어지는 지 분석한다. 정상 시나리오에서는 아래의 상황을 가정한다.


**비정상 시나리오**

비정상 시나리오에서는 계획된 uam 운항 스케줄을 따르지 않아 일정의 지연 혹은 수정이 예상되는 비정상적인 운행이 이루어질 때 시스템과 운영자 간 정보 교환이 올바르게 이루어지는 지 분석하며 2가지 비정상 시나리오로 세분화하였다.

첫 번째 비정상 시나리오는 uam 성능 문제 혹은 강풍 등 예상 가능한 내외부적 요인으로 예정된 스케줄 상 시간을 준수하지 못해 일정의 지연이 예상되지만 예정된 항로에 따라 예정된 vertiport까지 안전하게 운행할 수 있는 상황을 가정한다.

두 번째 비정상 시나리오는 예정된 스케줄 상의 시간은 준수하고 있지만 우발적인 내외부적 요인에 의해 예정된 vertiport까지 운행하지 못하고 일정 수정이 예상되는 상황을 가정한다.
