Project Plan: My Movie Tier List
Tool: AI Tutor (Model: Gemini 3.1 Flash Lite)
Communication: Conducted via this chat session
1. Target Audience
General movie fans.
2. Purpose
To organize personal movie preferences through a "Movie World Cup" (bracket-style voting) and to view the aggregated average rankings of all users.
3. UI and Functionality
Movie World Cup: A voting interface where users choose one of two movies.
Tier List: A drag-and-drop interface to categorize movies into specific tiers.
Data Integration:
User votes and tier placements are saved and persist after page refreshes.
Voting results are stored on a server (e.g., Firebase) to calculate and display real-time global average rankings.
Movie Data: Uses a pre-defined JSON file containing movie titles and poster image URLs.
4. Required Resources
JSON File: Contains movie titles and image URLs.
Backend Server: A database to store voting results and rankings (e.g., Firebase).
5. Definition of Done (Success Criteria)
Voting System: The Movie World Cup voting function works correctly.
Tier List: Movie cards can be moved into tier boxes using drag-and-drop.
Persistence: Movie cards remain in their assigned tiers after the page is refreshed.
Global Ranking: The system aggregates all users' votes and displays the real-time average ranking on the screen.
Data Loading: The pre-defined movie list from the JSON file loads correctly onto the webpage.

내 영화 티어리스트 (기획서)
작성 도구: AI 튜터 (모델: Gemini 3.1 Flash Lite)
대화 기록: 본 채팅창을 통해 진행함

누가 쓰나
불특정 다수의 영화 팬
무엇을 하려고
이상형 월드컵 방식의 투표를 통해 영화에 대한 내 취향을 정리하고, 전체 사용자의 평균 순위를 확인하기 위함
화면과 동작
이상형 월드컵: 두 영화 중 하나를 선택하는 투표 화면
티어리스트: 드래그 앤 드롭(Drag & Drop)을 통해 영화를 등급별로 배치하는 화면
데이터 연동:
사용자의 투표 및 배치 결과는 새로고침해도 유지됨
사용자의 투표 결과를 서버(Firebase 제안)에 저장하여 실시간 전체 평균 순위를 합산 및 출력
영화 목록: 미리 정해둔 영화 목록(JSON 파일)을 사용 (목록의 구체적인 영화 제목은 미정)
필요한 자료
영화 제목과 포스터 이미지 URL을 담은 JSON 파일
투표 결과 및 순위 데이터를 저장할 서버 (Firebase 등)
완성 기준
이상형 월드컵 투표 기능이 정상적으로 작동한다.
드래그 앤 드롭을 통해 영화 카드를 원하는 등급 칸으로 이동시킬 수 있다.
영화 카드를 특정 등급 칸에 배치하고 페이지를 새로고침해도 해당 위치에 그대로 남아 있다.
모든 사용자의 투표 결과를 합산하여 전체 평균 순위가 화면에 실시간으로 출력된다.
미리 정의된 JSON 파일의 영화 목록이 웹페이지에 정상적으로 로드된다.