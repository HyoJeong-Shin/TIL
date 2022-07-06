### PROJECT 03. 텍스트를 음성으로 변환하기   
---   

```
pip install gtts
pip install playsound
```

- gtts 라이브러리 : 문자를 음성으로 변환

- playsound 라이브러리 : mp3 파일을 파이썬에서 재생하기 위한 라이브러리 

- **'OSError: Unable to load sound named:' 오류 발생**  (main3-2.py) 
    - playsound 라이브러리를 사용할 때, 오디오 파일 경로에 공백이나 이모지가 포함되어 해당 오류 발생
    - 경로를 정상적으로 고치니 오류 해결
