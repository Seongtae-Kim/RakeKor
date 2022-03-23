# RakeKor

`RakeKor`는 `Rapid Automatic Keyword Extraction (RAKE)` 알고리즘의 한국어 버전입니다.

`RAKE` 알고리즘은 지도학습이 필요없이 빠르게 문서의 키워드를 뽑아낼 수 있습니다. TF-IDF와 같이 모든 문서에서의 단어들을 연산하지 않기 때문에 빠른 속도를 자랑합니다. 논문은 [여기](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.657.8134&rep=rep1&type=pdf)서 찾아볼 수 있습니다.

본래의 `RAKE` 알고리즘은 영어를 기반으로 제작되었지만 한국어의 교착어적인 특성에 의해 형태소는 공백을 단위로 보통 구분될 수 없기 때문에 `RakeKor`는 한국어의 특성을 고려 형태소 분석기에 의존하도록 튜닝을 거쳤습니다.

이 때문에  기본적인 형태소 분석기로 본 패키지에서는 파이썬 은전한닢 패키지를 사용합니다.

`RAKE`알고리즘과 마찬가지로 `RakeKor` 또한 두 개의 CONTENT 단어 사이에 위치하는 STOPWORD를 복합어로 정의하여 연산에 포함합니다.



> 예)
>
> ```
> 철수: CONTENT / 의: STOPWORD / 가방: CONTENT
> ```
>
> compound 파라미터를 True로 설정할 경우 두 개의 content 후보 키워드 사이에 있는 STOPWORD까지 붙여서 degree와 frequency를 계산합니다. False로 설정할 경우 연달아 이어지는 CONTENT 후보 키워드만 계산합니다.
>
> 

이후 사용자 정의 품사 태거를 추가할 수 있도록 업데이트를 진행할 예정입니다.





`RakeKor` is a Korean version of `Rapid Automatic Keyword Extraction (RAKE)` Algorithm. 

`RAKE` algorithm uses an unsupervised way to extract keywords from each document. Unlike TF-IDF, `RAKE` considers every document is independent. Thus, it shows much faster performance. You can find detailed information [here](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.657.8134&rep=rep1&type=pdf).

Unlike the original `RAKE` algorithm targeting English, Korean morphemes usually cannot be tokenized by spaces because it has an agglutinating morphology, which means that `RakeKor` must be dependent on morphological tagger.

For this, the default morphological(pos) tagger I used on this package is [python eunjeon](https://github.com/koshort/pyeunjeon).

Like `RAKE` algorithm, `RakeKor` also considers candidate keywords whose STOPWORD is between the two CONTENT words.

> Ex)
>
> ```
> 철수: CONTENT / 의: STOPWORD / 가방: CONTENT
> ```
>
> If you set True on compound parameter, the model also considers degrees and frequencies of additional pattern (CONTENT-STOPWORD-CONTENT) of candidate keywords. If False, the model only regards consecutive CONTENTs.

Futher updates will be available to add custom POS taggers.



## Install Dependencies

```bash
pip3 install -r requirements.txt
```





## Parameters

- [--source_texts_dir]

  텍스트 파일이 위치하고 있는 폴더의 경로 (기본값: samples/)

- [--output_dir]

  결과 엑셀파일(.xlsx)을 저장할 경로 (기본값: output/)

- [--compound]

  복합어를 포함할지 여부 (기본값: False)

- [--min_length]

  키워드를 구성하는 최소 토큰 길이

- [--max_length]

  키워드를 구성하는 최대 토큰 길이

- [--show result]

  터미널에 중간 데이터프레임을 표시할지 여부 (기본값: True)

  

- [--source_texts_dir]

  Targeting text source directory (default: samples/)

- [--output_dir]

  Targeting result output directory (default: output/)

- [--compound]

  Whether the algorithm considers compounds or not (default: False)

- [--min_length]

  Minimum token lengths for keywords

- [--max_length]

  Maximum token lengths for keywords

- [--show result]

  Showing intermediate dataframe result on terminal (default: True)







## Sample Result

```
민간 주도 소형발사체 개발 지원
2027년까지 278억원 투입키로


과학기술정보통신부가 민간 기업 주도로 2단형 소형발사체를 개발할 수 있도록 지원한다. 미국 항공우주국(NASA) 도움으로 스페이스X의 팰컨9이 개발됐듯 ‘한국형 스페이스X’를 키울 계획이다.

과기부는 2027년까지 6년간 278억5000만원을 투입해 ‘소형 발사체 개발역량 지원사업’에 착수한다고 16일 밝혔다. 누리호의 75t 엔진을 1단으로 하고, 2단 엔진은 민간 기업 주도로 개발해 2단형 소형 발사체를 완성한다.

과기부에 따르면 민간 기업이 우주개발을 주도하면서 세계적으로 소형 발사체 수요가 증가하고 있다. 그러나 국내에 소형 발사체가 없다보니 해외 중대형 위성의 발사 일정이 정해져야 국내 소형 위성도 남는 기간에 발사가 가능한 실정이다.

과기부는 스페이스X를 우주기업으로 육성한 미 나사의 ‘상업용 궤도 수송 서비스 프로젝트(COTS)’ 방식을 본떠 경제성을 갖춘 소형 발사체 기업을 키우기로 했다. COTS는 기업이 단계별 목표를 이루면 정부가 개발금을 제공하는 식으로, 이를 통해 스페이스X의 대표 발사체인 팰컨9이 개발됐다.

지원 대상은 한국 국적 우주 산업체이며 공모 기간을 거쳐 4월 중 3개 기업을 선정해 본격 연구를 시작한다.
```



| **length** |          **keyword**           |    **score**     |
| :--------: | :----------------------------: | :--------------: |
|     6      | 민간 주도 소형발사체 개발 지원 |      13.625      |
|     6      |  소형발사체 개발역량 지원사업  |      12.125      |
|     4      |       과학기술정보통신부       |        10        |
|     4      |     한국 국적 우주 산업체      |       8.5        |
|     3      |        단형 소형발사체         |        8         |
|     3      |         민간 기업 주도         | 6.78571428571429 |
|     3      |        소형발사체 기업         | 6.28571428571429 |
|     3      |        소형발사체 수요         |        6         |
|     3      |         국내 소형 위성         |        6         |
|     2      |           소형발사체           |        5         |
|     2      |           민간 기업            | 4.78571428571429 |
|     2      |          대표 발사체           |        4         |
|     2      |            우주개발            |      3.625       |
|     2      |            우주기업            | 3.28571428571429 |
|     2      |        미국 항공우주국         |        3         |
|     2      |           스페이스X            |        3         |







