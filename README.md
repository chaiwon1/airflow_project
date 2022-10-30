<h1>Simple ETL DAG Project</h1>

Docker Compose를 이용해 컨테이너를 관리하였습니다.
Airflow의 MetastoreDB는 Postgres입니다. 

1. `Weather_to_Redshift` : <a href="https://openweathermap.org">Open Weather API</a>에서 조회일로부터 8일간의 서울 지역의 예상 기상정보를 조회하여 redshift에 적재

2. `MySQL_to_Redshift` : MySQL에 있는 데이터를 S3에 저장 후, Redshift에 적재


사용스킬 : `AWS Redshift`, `AWS S3`, `MySQL`, `Postgres`, `Docker`

<br>

<h2>프로젝트 아키텍쳐</h2>

<h4>Weather_to_Redshift</h4>

<ul>
  <li>Open API로부터 Extract 한다.</li>
  <li>json형식으로 불러온 데이터를 필요한 정보인 낮/최소/최대 온도 데이터만 Transform 한다.</li>
  <li>Redshift에 <b>Full Refresh</b>방식과 <b>Incremental Update</b>방식으로 Load 한다. </li>
    <ul>
      <li>Full Refresh DAG : Weather_to_Redshift.py</li>
        <ul>
          <li>start_date : 2022년 1월 1일</li>
          <li>schedule_interval : 매일 02:00 AM 
        </ul>
      <li>Incremental Refresh DAG : Weather_to_Redshift_2.py</li>
        <ul>
          <li>start_date : 2022년 1월 1일</li>
          <li>schedule_interval : 매일 04:00 AM 
        </ul>
    </ul>
</ul>



<br>

<h4>MySQL_to_Redshift</h4>

<ul>
  <li>가상의 Production DB인 MySQL에서 데이터를 Extract 한다.</li>
  <li>데이터를 S3에 Load 한다.</li>
  <li>그 뒤, DW인 Redshift로 다시 Load 한다.</li>
</ul>

<img alt="Untitled (1)" src="https://user-images.githubusercontent.com/95471902/198871869-d36f4725-2879-42f1-8aad-c2deb4f251e8.png">
