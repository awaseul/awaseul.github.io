<a href="https://colab.research.google.com/github/awayeseullee/awayeseullee/blob/main/%EC%A0%9C%EC%A1%B0_%EA%B8%88%EC%9C%B5_%EA%B2%8C%EC%9E%84_%EB%8F%84%EB%A9%94%EC%9D%B8_%EB%B6%84%EC%84%9D_%EB%B0%8F_%EC%8B%9C%EA%B0%81%ED%99%94_EDA_practice.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>



## **🔒 [제조] Practice**
---
* **제조 데이터(Time Series, 시계열)를 대상으로 분석 및 시각화 하기**

* 데이터 명세 ⬇

|Column|Description|
|:---|:---|
|datetime|시간|
|Accelerometer1RMS|진동 가속도1|
|Accelerometer2RMS|진동 가속도2|
|Current |전기모터 암페어(Ampere)|
|Pressure|워터 펌프 후 루프의 압력(Bar) |
|Temperature|엔진의 온도(섭씨 온도)|
|Thermocouple|순환 루프에서 유체의 온도(섭씨 온도)|
|Voltage|전기 모터의 전압(Volt)|
|RateRMS|루프 내부의 유체의 순환 유량(Liter/min)|
|anomaly|이상여부|
|changepoint|변경점 여부|


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive



```python
import pandas as pd
df = pd.read_csv('/content/drive/MyDrive/[패캠] 데이터분석 강의/[패캠] 데이터분석 과제_4주차/example_1.csv', sep=";")
df.head()
```





  <div id="df-c576c8ab-7087-4635-be41-232d1db3017d" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>datetime</th>
      <th>Accelerometer1RMS</th>
      <th>Accelerometer2RMS</th>
      <th>Current</th>
      <th>Pressure</th>
      <th>Temperature</th>
      <th>Thermocouple</th>
      <th>Voltage</th>
      <th>Volume Flow RateRMS</th>
      <th>anomaly</th>
      <th>changepoint</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-03-09 16:16:30</td>
      <td>0.027545</td>
      <td>0.041127</td>
      <td>0.673506</td>
      <td>0.054711</td>
      <td>67.8345</td>
      <td>24.3164</td>
      <td>240.513</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-03-09 16:16:31</td>
      <td>0.027997</td>
      <td>0.039100</td>
      <td>0.772264</td>
      <td>0.054711</td>
      <td>67.8704</td>
      <td>24.3279</td>
      <td>229.523</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-03-09 16:16:32</td>
      <td>0.028418</td>
      <td>0.038872</td>
      <td>0.675520</td>
      <td>0.054711</td>
      <td>67.7882</td>
      <td>24.3261</td>
      <td>242.708</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-03-09 16:16:33</td>
      <td>0.027625</td>
      <td>0.039366</td>
      <td>0.566279</td>
      <td>-0.273216</td>
      <td>67.7918</td>
      <td>24.3323</td>
      <td>229.709</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-03-09 16:16:34</td>
      <td>0.027484</td>
      <td>0.041854</td>
      <td>1.292170</td>
      <td>0.054711</td>
      <td>67.7368</td>
      <td>24.3250</td>
      <td>242.746</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-c576c8ab-7087-4635-be41-232d1db3017d')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-c576c8ab-7087-4635-be41-232d1db3017d button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-c576c8ab-7087-4635-be41-232d1db3017d');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-90462c66-f74c-434f-8f9b-742ddbc469a0">
  <button class="colab-df-quickchart" onclick="quickchart('df-90462c66-f74c-434f-8f9b-742ddbc469a0')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-90462c66-f74c-434f-8f9b-742ddbc469a0 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>




### Question 01

```
Data Read하고 상위 전처리 조건을 적용한 DataFrame 만들기

  (1) Data shape(형태) 출력 → 전체 데이터의 Row와 Column개수 출력

  (2) Data type 확인 → 각 Column별 Data Type 출력

  (3) Null값 확인 (※ 빈 값의 Data) → 각 Column별 Null Value의 개수 출력
```


```python
df.describe()
```





  <div id="df-fa673767-d028-44cf-b23d-0fd6b854e9d3" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Accelerometer1RMS</th>
      <th>Accelerometer2RMS</th>
      <th>Current</th>
      <th>Pressure</th>
      <th>Temperature</th>
      <th>Thermocouple</th>
      <th>Voltage</th>
      <th>Volume Flow RateRMS</th>
      <th>anomaly</th>
      <th>changepoint</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
      <td>1063.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.027663</td>
      <td>0.040037</td>
      <td>1.195240</td>
      <td>0.048541</td>
      <td>67.895174</td>
      <td>24.265096</td>
      <td>229.924824</td>
      <td>31.472275</td>
      <td>0.313264</td>
      <td>0.003763</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.000333</td>
      <td>0.001086</td>
      <td>7.113407</td>
      <td>0.270689</td>
      <td>0.695198</td>
      <td>0.021573</td>
      <td>13.259556</td>
      <td>1.314535</td>
      <td>0.464039</td>
      <td>0.061256</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.026455</td>
      <td>0.036972</td>
      <td>0.394058</td>
      <td>-0.929070</td>
      <td>66.201900</td>
      <td>24.217000</td>
      <td>0.580776</td>
      <td>28.040000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.027434</td>
      <td>0.039276</td>
      <td>0.753505</td>
      <td>0.054711</td>
      <td>67.412450</td>
      <td>24.252950</td>
      <td>223.570000</td>
      <td>31.039050</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.027674</td>
      <td>0.040098</td>
      <td>1.002720</td>
      <td>0.054711</td>
      <td>67.955000</td>
      <td>24.267200</td>
      <td>230.634000</td>
      <td>32.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.027892</td>
      <td>0.040828</td>
      <td>1.194580</td>
      <td>0.054711</td>
      <td>68.515500</td>
      <td>24.277450</td>
      <td>236.960000</td>
      <td>32.038900</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.028554</td>
      <td>0.043122</td>
      <td>232.734000</td>
      <td>1.038490</td>
      <td>69.098200</td>
      <td>24.332300</td>
      <td>254.125000</td>
      <td>33.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-fa673767-d028-44cf-b23d-0fd6b854e9d3')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-fa673767-d028-44cf-b23d-0fd6b854e9d3 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-fa673767-d028-44cf-b23d-0fd6b854e9d3');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-691b7fc4-862f-489f-ad95-420a405c1b17">
  <button class="colab-df-quickchart" onclick="quickchart('df-691b7fc4-862f-489f-ad95-420a405c1b17')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-691b7fc4-862f-489f-ad95-420a405c1b17 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
# 1번_Data shape(형태) 출력 → 전체 데이터의 Row와 Column개수 출력

df.shape
```




    (1063, 11)




```python
# 2번_Data type 확인 → 각 Column별 Data Type 출력

df.dtypes
```




    datetime                object
    Accelerometer1RMS      float64
    Accelerometer2RMS      float64
    Current                float64
    Pressure               float64
    Temperature            float64
    Thermocouple           float64
    Voltage                float64
    Volume Flow RateRMS    float64
    anomaly                float64
    changepoint            float64
    dtype: object




```python
# df.info() -> 데이터 타입, Null값 함께 확인 가능 + 기본적인 데이터 현황 한 번에 확인 가능

df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1063 entries, 0 to 1062
    Data columns (total 11 columns):
     #   Column               Non-Null Count  Dtype  
    ---  ------               --------------  -----  
     0   datetime             1063 non-null   object 
     1   Accelerometer1RMS    1063 non-null   float64
     2   Accelerometer2RMS    1063 non-null   float64
     3   Current              1063 non-null   float64
     4   Pressure             1063 non-null   float64
     5   Temperature          1063 non-null   float64
     6   Thermocouple         1063 non-null   float64
     7   Voltage              1063 non-null   float64
     8   Volume Flow RateRMS  1063 non-null   float64
     9   anomaly              1063 non-null   float64
     10  changepoint          1063 non-null   float64
    dtypes: float64(10), object(1)
    memory usage: 91.5+ KB



```python
# 3번_Null값 확인 (※ 빈 값의 Data) → 각 Column별 Null Value의 개수 출력

df.isnull().sum()
```




    datetime               0
    Accelerometer1RMS      0
    Accelerometer2RMS      0
    Current                0
    Pressure               0
    Temperature            0
    Thermocouple           0
    Voltage                0
    Volume Flow RateRMS    0
    anomaly                0
    changepoint            0
    dtype: int64



### Question 02

```
01번 문제에서 Read한 데이터를 활용하여 시각화 진행하기

  (1) 전체 데이터의 개수에서 'anomaly'가 차지하는 비율
  
  (2) 'Accelerometer1RMS','Accelerometer2RMS','Current','Pressure','Temperature','Thermocouple','Voltage','Volume Flow RateRMS'
  총 8개의 Column 대상으로 총 8개의 Trend 그래프 시각화
  (※ x = 'datetime', y= 각 Column)

  (3) 시각화한 Trend 그래프 위에 'anomaly'가 1인 데이터에 대해서 이상 포인트를 표시하기

```

### 전체 데이터의 개수에서 'anomaly'가 차지하는 비율 **약 31%**


```python
df[df["anomaly"]==1]

# anomaly 개수 = 333개
```





  <div id="df-0b958eb9-6874-4c92-8521-39947bcf417b" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>datetime</th>
      <th>Accelerometer1RMS</th>
      <th>Accelerometer2RMS</th>
      <th>Current</th>
      <th>Pressure</th>
      <th>Temperature</th>
      <th>Thermocouple</th>
      <th>Voltage</th>
      <th>Volume Flow RateRMS</th>
      <th>anomaly</th>
      <th>changepoint</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>560</th>
      <td>2020-03-09 16:26:30</td>
      <td>0.027489</td>
      <td>0.040797</td>
      <td>1.162620</td>
      <td>-0.273216</td>
      <td>67.4538</td>
      <td>24.2726</td>
      <td>231.836</td>
      <td>32.0000</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>561</th>
      <td>2020-03-09 16:26:31</td>
      <td>0.028236</td>
      <td>0.040921</td>
      <td>0.478813</td>
      <td>0.382638</td>
      <td>67.5373</td>
      <td>24.2795</td>
      <td>219.732</td>
      <td>31.9615</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>562</th>
      <td>2020-03-09 16:26:32</td>
      <td>0.028028</td>
      <td>0.040447</td>
      <td>1.212860</td>
      <td>0.054711</td>
      <td>67.6272</td>
      <td>24.2728</td>
      <td>230.915</td>
      <td>31.0397</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>563</th>
      <td>2020-03-09 16:26:33</td>
      <td>0.027633</td>
      <td>0.041229</td>
      <td>0.953656</td>
      <td>0.382638</td>
      <td>67.6784</td>
      <td>24.2831</td>
      <td>215.346</td>
      <td>32.0396</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>564</th>
      <td>2020-03-09 16:26:34</td>
      <td>0.027954</td>
      <td>0.040867</td>
      <td>1.139300</td>
      <td>0.054711</td>
      <td>67.6033</td>
      <td>24.2773</td>
      <td>219.743</td>
      <td>32.9615</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>888</th>
      <td>2020-03-09 16:33:25</td>
      <td>0.027327</td>
      <td>0.039147</td>
      <td>0.846175</td>
      <td>0.710565</td>
      <td>66.4771</td>
      <td>24.2400</td>
      <td>238.852</td>
      <td>32.0404</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>889</th>
      <td>2020-03-09 16:33:26</td>
      <td>0.027102</td>
      <td>0.037518</td>
      <td>1.250500</td>
      <td>0.054711</td>
      <td>66.5466</td>
      <td>24.2347</td>
      <td>225.364</td>
      <td>32.9608</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>890</th>
      <td>2020-03-09 16:33:27</td>
      <td>0.027542</td>
      <td>0.037979</td>
      <td>1.233640</td>
      <td>0.054711</td>
      <td>66.3557</td>
      <td>24.2300</td>
      <td>222.442</td>
      <td>32.0000</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>891</th>
      <td>2020-03-09 16:33:28</td>
      <td>0.027083</td>
      <td>0.038606</td>
      <td>1.333360</td>
      <td>0.054711</td>
      <td>66.5840</td>
      <td>24.2407</td>
      <td>251.784</td>
      <td>32.0000</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>892</th>
      <td>2020-03-09 16:33:29</td>
      <td>0.026769</td>
      <td>0.040216</td>
      <td>1.168110</td>
      <td>-0.273216</td>
      <td>66.2921</td>
      <td>24.2363</td>
      <td>216.306</td>
      <td>32.0000</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>333 rows × 11 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-0b958eb9-6874-4c92-8521-39947bcf417b')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-0b958eb9-6874-4c92-8521-39947bcf417b button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-0b958eb9-6874-4c92-8521-39947bcf417b');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-e9ca4ad1-f4ab-4c17-9368-93b088ef537f">
  <button class="colab-df-quickchart" onclick="quickchart('df-e9ca4ad1-f4ab-4c17-9368-93b088ef537f')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-e9ca4ad1-f4ab-4c17-9368-93b088ef537f button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
333/len(df)
# 약 31%
```




    0.31326434619002824




```python
# (normalize=True) : 비율로 바로 계산할 때 사용

df['anomaly'].value_counts(normalize=True)
```




    0.0    0.686736
    1.0    0.313264
    Name: anomaly, dtype: float64



### 8개 Trend graph 시각화 및 anomaly 표시


```python
import matplotlib.pyplot as plt


df['datetime'] = pd.to_datetime(df['datetime'])


for v, i in  enumerate(df.columns[1:9]) :
  plt.figure(figsize=(24,15))
  plt.subplot(8, 1, v+1)
  plt.plot(df['datetime'], df[i], color='grey');
  plt.plot(df[df['anomaly']==1]['datetime'], df[df['anomaly']==1][i], 'o', color='red', markersize=3 );
  plt.title(i)
```


    
![png](output_17_0.png)
    



    
![png](output_17_1.png)
    



    
![png](output_17_2.png)
    



    
![png](output_17_3.png)
    



    
![png](output_17_4.png)
    



    
![png](output_17_5.png)
    



    
![png](output_17_6.png)
    



    
![png](output_17_7.png)
    



```python
# Current 1002행 "232.734000"으로 anomaly는 아니었으나 이상치 데이터로 간주하여 삭제 후 그래프 생성함

df.drop(df.index[1002], axis=0, inplace=True)
```


```python
import seaborn as sns
df['datetime'] = pd.to_datetime(df['datetime'])

# Plotting the trend graph
plt.figure(figsize=(12, 6))
sns.lineplot(x='datetime', y='Current', data=df, label='Trend of Current')

# Highlighting anomalies
anomaly_points = df[df['anomaly'] == 1]
plt.scatter(anomaly_points['datetime'], anomaly_points['Current'], color='red', label='Anomalies')

# Customizing the plot
plt.title('Current Trend with Anomalies', pad=10)
plt.xlabel('Datetime')
plt.ylabel('Current')
plt.legend()
#plt.grid(True)
plt.show()
```


    
![png](output_19_0.png)
    

