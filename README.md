# 🎥 영화 리뷰

## 결국에 내가 무엇을 하려고 하는 거지?

> 먼저 이 앱의 핵심 기능 사이클은 어떻게 동작되는지 확인해보자

- 사용자는 메인 페이지에서 지금 인기 있는 영화 목록을 확인할 수 있다.
- 헤더의 검색창에서 키워드를 검색하면 키워드에 맞는 영화 목록을 확인할 수 있다.
- 이때 스크롤을 내리면 추가적으로 데이터를 패칭해 계속해서 영화 목록을 부른다.
- 하나의 영화를 누르면 자세한 정보를 (카테고리, 별점, 줄거리 등) 확인할 수 있다.
- 사용자는 내 별점 칸에서 영화에 대한 자신의 별점을 남길 수 있다.

## 핵심 기능을 1줄로 정의해 보기

사용자가 영화 정보를 탐색하고 개인별 별점을 남길 수 있다.

## 동작 가능한 가장 작은 버전부터 시뮬레이션 해보기

> 2에서 작성한 핵심 기능 1줄을 생각하며 동작 가능한 가장 작은 버전을 위한 단계를 구성해 보기. **단, 핵심을 포함하게.**

1. 초기 영화 목록 불러오기
2. 검색어 입력 시 검색어에 해당하는 영화 목록 불러오기
3. 영화 목록이 무한 스크롤 되도록 추가 영화 목록 불러오기
4. 하나의 영화에 대한 상세 정보 모달 생성하기
5. 상세 정보 모달에서 별점을 부여할 수 있도록 내 별점 칸 생성하기

### 🤖 From. GPT

1. 메인 페이지 구성

   - UI 디자인 및 기본 레이아웃 설정.
   - 인기 영화 목록을 보여주는 카드 컴포넌트 제작.

2. 영화 데이터 패칭

   - 외부 API 연동 및 데이터 가져오기.
   - 초기 영화 목록 데이터 요청 및 렌더링.
   - 무한 스크롤 기능 구현 (스크롤 시 추가 데이터 요청 및 목록 업데이트)

3. 검색 기능 구현

   - 검색창 컴포넌트 추가.
   - 키워드 입력 시 API 호출하여 검색 결과 반환 및 목록 갱신.

4. 영화 상세 페이지

   - 영화 상세 페이지 레이아웃 디자인.
   - 영화 클릭 시 상세 페이지로 이동하는 라우팅 설정.
   - 영화 상세 정보 API 호출 및 데이터 표시.

5. 별점 기능 추가
   - 별점 입력 컴포넌트 제작.
   - 사용자가 별점 입력 시 해당 데이터를 저장하는 기능 구현.
   - 저장된 별점을 표시하는 기능 추가.

## 핵심과 가까우면서 쉽게 할 수 있는 하나를 선택하여 문제를 쪼개기

> 초기 영화 목록 불러오기

1. 영화 카드 컴포넌트 UI 디자인 및 기본 레이아웃 구현 (Storybook)
2. 영화 카드 컴포넌트로 전체 레이아웃 구현
3. 영화 목록 패칭 API 구성
4. 영화 목록 정상 패칭 여부 테스트
5. 초기 영화 목록 데이터 요청 밒 렌더링

### 🤖 From. GPT

1. API 연동 준비

   - 외부 영화 데이터 API(예: TMDb, OMDb) 키 발급 및 설정.
   - API 호출을 위한 기본 설정 (기본 URL, API 키 등) 구성.

2. API 호출 함수 작성

   - API 호출을 위한 함수 작성 (예: fetchPopularMovies).
   - HTTP 클라이언트를 사용하여 API 호출 및 데이터 받기.

3. 메인 페이지 컴포넌트 작성

   - 메인 페이지 레이아웃 작성.
   - 영화 목록을 표시할 컴포넌트 작성.

4. API 데이터 연동

   - 메인 페이지 로드 시 API 호출하여 영화 데이터 가져오기.
   - 가져온 데이터를 상태에 저장.

5. 영화 목록 렌더링

   - 상태에 저장된 영화 데이터를 기반으로 목록 렌더링.
   - 각 영화에 대한 카드 컴포넌트 제작 및 리스트로 표시.

6. 에러 처리 및 로딩 상태 관리

   - API 호출 시 발생할 수 있는 에러 처리 (예: 네트워크 오류, 데이터 없음 등).
   - API 호출 중 로딩 상태를 표시하는 UI 추가 (로딩 스피너 등).

## 결과를 만드는데 필요한 과정을 구체화하고 최대한 진짜처럼 시뮬레이션 해보기

| 단계 | 기능                                                           | 상태   | 상태 관리 로직                                                  | 커스텀 훅                         |
| ---- | -------------------------------------------------------------- | ------ | --------------------------------------------------------------- | --------------------------------- |
| 1    | 초기 영화 목록 불러오기                                        | movies | 초기 상태로 빈 배열 설정, API 호출 후 영화 데이터를 상태에 저장 | useFetchMovies                    |
| 2    | 검색어 입력 시 검색어에 해당하는 영화 목록 불러오기            | movies | 검색 키워드 변경 시 API 다시 호출, 결과를 상태에 저장           | useFetchMovies                    |
| 3    | 영화 목록이 무한 스크롤 되도록 추가 영화 목록 불러오기         | movies | 스크롤 시 API로 다음 페이지 다시 호출, 결과를 상태에 저장       | useFetchMovies, useInfiniteScroll |
| 4    | 하나의 영화에 대한 상세 정보 모달 생성하기                     | isOpen | 영화 카드 컴포넌트 클릭 시 상세 모달 오픈 상태로 변경           | useModal                          |
| 5    | 상세 정보 모달에서 별점을 부여할 수 있도록 내 별점 칸 생성하기 | rating | 별점 입력 시 상태에 별점 저장, 제출 시 API 호출                 | useRating                         |
|      |
