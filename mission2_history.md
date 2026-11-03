# 미션 2 기록

- 사용한 AI 이름 / 대화 공유 링크 또는 캡처 파일명:
  Gemini 3.1 / [mission2_log.md](./mission2_log.md)
  

- 좁혀서 적은 한 문장: I need to drag and drop so It need to make a something to move but there is no tile or box with movie name. 영화 카드(Movie Card)가 화면에 보이지 않아서 드래그할 대상 자체가 없다.

- 좁히려고 해 본 것과 결과:

현재 코드의 가장 큰 문제는 saveState 함수가 HTML 구조(태그) 자체를 저장하고, initApp이 이를 다시 불러올 때 이벤트 리스너(드래그 기능)가 연결되지 않은 상태로 화면에 그려지기 때문에 발생하는 문제입니다. initApp 함수에서 movies 배열을 순회하며 카드를 만드는 로직이 else 문(데이터가 없을 때) 안에만 갇혀 있기 때문입니다. 현재 코드는 localStorage에 데이터가 있으면 그것만 불러오고, 없으면 새로 만듭니다. 하지만 localStorage에 데이터가 있더라도 **"어떤 영화가 있는지"**에 대한 정보가 없다면 카드를 그릴 수 없습니다(초기화 로직의 분리). 영화 정보(데이터)와 화면에 그리는 코드(DOM 생성)가 섞여 있어서, 데이터가 있으면 화면을 새로 그려야 하는데 그 과정이 빠져 있습니다(데이터와 뷰의 분리 부족).

**"데이터를 기준으로 카드를 생성하는 함수"**를 따로 만드는 것이 좋습니다.

수정된 핵심 로직 (이 부분을 확인해 보세요)
저장 방식 변경: HTML 태그가 아닌, **"어떤 영화가 어떤 티어에 있는지"**에 대한 정보만 객체로 저장합니다.
재생성 로직: 페이지가 로드될 때, 저장된 데이터를 바탕으로 카드를 **새로 생성(createElement)**하고 이벤트를 다시 연결합니다.

// 1. 수정된 저장 함수: HTML이 아닌 데이터(어디에 어떤 영화가 있는지)를 저장
function saveState() {
    const state = {
        S: Array.from(document.getElementById('S').children).map(c => c.id),
        A: Array.from(document.getElementById('A').children).map(c => c.id),
        pool: Array.from(document.getElementById('movie-pool').children).map(c => c.id)
    };
    localStorage.setItem('tierState', JSON.stringify(state));
}

// 2. 카드 생성 함수 (이벤트를 매번 연결해 줍니다)
function createCard(movie) {
    const card = document.createElement('div');
    card.className = 'movie-card';
    card.draggable = true;
    card.id = movie.id;
    card.innerText = movie.title;
    card.ondragstart = drag; // 드래그 이벤트 연결
    return card;
}

// 3. 수정된 initApp: 데이터를 읽어와서 카드를 새로 만듭니다.
function initApp() {
    const saved = localStorage.getItem('tierState');
    const state = saved ? JSON.parse(saved) : { S: [], A: [], pool: movies.map(m => m.id) };

    // 각 컨테이너에 카드를 배치
    ['S', 'A', 'pool'].forEach(tierId => {
        const container = document.getElementById(tierId);
        state[tierId].forEach(movieId => {
            const movie = movies.find(m => m.id === movieId);
            if (movie) container.appendChild(createCard(movie));
        });
    });
}

이벤트 유지: innerHTML로 HTML 문자열을 넣으면 브라우저는 그 안의 자바스크립트 함수 연결을 기억하지 못합니다. 하지만 createCard 함수를 통해 직접 요소를 만들고 ondragstart = drag를 할당하면, 브라우저는 이 카드가 드래그 가능하다는 것을 명확히 인지합니다.
데이터 무결성: HTML 구조가 바뀌어도 데이터(JSON)는 그대로 유지되므로, 나중에 서버(Firebase)와 연동할 때 훨씬 유리합니다.

- AI가 말한 원인과, 그 설명을 직접 확인한 결과:

1)
에러 메시지인 **Uncaught TypeError: Cannot set properties of null (setting 'innerHTML')**는 자바스크립트가 document.getElementById로 요소를 찾으려 했지만, 해당 ID를 가진 HTML 요소를 찾지 못했다는 뜻

- 스크립트가 HTML보다 먼저 실행되고 있을 가능성.

HTML 파일의 <body> 태그 안에 아래와 같은 구조가 있는지 확인하세요. 특히 **id="pool"**이 맞는지 확인하세요. (이전 코드에서는 movie-pool이었는데, initApp 코드에서는 pool로 되어 있을 수 있습니다.)

2) 
forEach를 호출하는 대상인 **state[tierId]가 undefined**인 상태

아주 미세한 데이터 키(Key) 불일치 문제입니다.

원인 분석
initApp 함수에서 state 객체를 가져올 때, state 객체 안에는 S, A, pool이라는 키가 있습니다. 하지만 tiers 배열에서 movie-pool이라는 ID를 사용하고 있죠.


파일명	고친 내용	다시 해 본 결과
Entering_Movie_A_updated2.html	1. saveState에서 HTML 태그가 아닌 ID 배열만 저장하도록 변경
2. createCard 함수를 별도로 분리하여 드래그 이벤트 재연결 로직 구현
3. initApp에서 데이터를 기반으로 카드를 매번 새로 생성하도록 로직 재설계
4. HTML ID(movie-pool)와 데이터 키(pool) 간의 매핑 불일치 해결	성공. 에러 없이 화면에 영화 카드가 정상적으로 렌더링되며, 드래그앤드롭 및 새로고침 후에도 상태가 완벽하게 유지됨.

- (선택) 코랩에서 함께 이야기하고 싶은 내용과 그 이유:

-  "왜 이런 문제가 발생했는지" 근본적인 원인(이벤트 리스너 소실, 데이터-뷰 분리)
- Data centered Design