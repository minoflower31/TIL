# I/O Stream

- 네트워크에서 자료의 흐름이 물의 흐름과 같다는 비유에서 유래
- 자바는 I/O Stream을 통해 다양한 입출력 장치로부터 독립적으로 일관성있는 I/O를 제공
- e.g. 키보드, 마우스, 네트워크, 메모리 등

<br>

c.f. 표준 입출력 스트림은 `System` 클래스를 가지고 있음 <br>
문자 입출력 스트림은 `Reader`나 `Writer`가 포함된 클래스<br>
바이트 입출력 스트림은 `InputStream` 이나 `OutputStream` 가 포함된 클래스 
<br>

## 구분
### Input Stream / Output Stream

    입력 스트림: FileInputStream, FileReader, BufferedReader, BufferedInputStream...

    출력 스트림: FileOutputStream, FileWriter, BufferedWriter...
<br>

### 문자 단위 스트림 / 바이트 단위 스트림
    문자 단위 스트림: 바이트 단위로 처리하면 한글 깨짐. 인코딩에 맞게 2바이트 이상인 문자를 처리하도록 구현됨
        e.g. FileReader, FileWriter, BufferedReader, BufferedWriter...

    바이트 단위 스트림: 동영상, 음악파일, 실행 파일 등의 자료를 읽거나 쓸 때
        e.g. File || Buffered - InputStream || OutputStream
<br>


### 기반 스트림 / 보조 스트림
    기반 스트림: 대상에 직접적으로 자료를 읽고 쓰는 스트림
        e.g. FileInputStream, FileOutputStream, FileReader, FileWriter...
    
    보조 스트림: 기반 스트림에 추가적인 기능을 더해주는 스트림
        e.g. InputStreamReader, BufferedOutputStream...
    
<br>
<br>

## 바이트 단위 I/O 스트림

### InputStream

- **바이트 단위 입력 스트림의 최상위 추상 클래스**
- 주요 하위 클래스
  - **`FileInputStream`** (파일을 바이트 단위로 읽음)
  - `ByteArrayInputStream` (byte 배열을 바이트 단위로)
  - `FilterInputStream` (보조 스트림의 상위 클래스)

- 주요 메서드
  - read(), read(byte[] b)...

<br>

### OutputStream

- 바이트 단위 출력 스트림의 최상위 추상 클래스
- 주요 하위 클래스
  - **`FileOutputStream`** -> 두 번째 인자로 true를 전달하면 append. 
  - `ByteArrayOutputStream`
  - `FilterOutputStream`

- 주요 메서드
  - write()...
 
<br>
  
#### flush()와 close() 메서드

close() 메서드 내부에 flush()가 들어있음.

<br>

## 문자 단위 I/O 스트림

### Reader

- 문자 단위 입력 스트림의 최상위 추상 클래스
- 주요 하위 클래스
  - `FileReader` (파일에서 문자 단위로 읽는 스트림 클래스)
  - `InputStreamReader`
  - `BufferedReader`

<br>

### Writer

- 문자 단위 출력 스트림의 최상위 추상 클래스
- 주요 하위 클래스
  - `FileWriter`
  - `OutputStreamWriter`
  - `BufferedWriter`

<br>

# 보조 스트림

- 직접적으로 읽고 쓰는 스트림이 아닌 **보조 기능을 제공하는 스트림**
- 상위 클래스는 `FilterInputStream`과 `FilterOutputStream`
- **생성자의 매개변수로 또 다른 스트림을 가짐**
- Decorator 패턴으로 구현됨 (감싸는 형태)

<br>

<img width="561" alt="image" src="https://user-images.githubusercontent.com/56334513/163795861-258f990e-9633-41e8-8708-8534da332e1b.png">

<br>

## InputStreamReader, OutputStreamWriter

- **바이트** 단위로 읽거나 쓰는 자료를 **문자**로 변환해주는 보조 스트림
- e.g. FileInputStream로 읽은 바이트를 문자로 변환
  - `InputStreamReader isr = new InputStreamReader(new FileInputStream("a.txt"))`

<br>

## BufferedInputStream, BufferedOutputStream

- 약 8k의 배열이 제공되어 입출력을 빠르게 하는 기능이 제공되는 보조 스트림
- BufferedReader와 BufferedWriter는 문자용 입출력 보조 스트림!

<br>
<br>

# Serialization (직렬화)

- 인스턴스 상태를 그대로 **파일로 저장**하거나 **네트워크로 전송**하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정
- 반대로 일련의 바이트로부터 데이터 구조를 추출하는 과정은 `Deserialization` 
- 자바에서는 보조 스트림을 이용: `ObjectInputStream`, `ObjectOutputStream`

<br>

## Serializable 인터페이스

- **인터페이스를 표시하는 이유**
  - 직렬화는 인스턴스의 내용이 외부로 유출되는 것이기 때문에 프로그래머가 해당 객체에 대한 직렬화 의도를 표시해야 함
- **구현 코드가 없는 marker interface**
- `transient` 키워드: 직렬화하지 않으려는 멤버 변수에 사용 (e.g. Socket 객체는 직렬화하지 못함)

<br>

## Externalizable 인터페이스

- `writerExternal()`과 `readExternal()` 메서드를 구현해야 함
- 프로그래머가 직접 객체를 읽고 쓰는 코드를 구현함 (대부분 Serializable로 커버가능하지만 지원하지 않는 기능을 구현해야 할 때 사용)

<br>

# File 관련 클래스

## File

- 파일 개념을 추상화한 클래스
- 파일의 이름, 경로, 읽기 전용 등의 속성을 알 수 있음
- 단, 입출력 기능 X

<br>

## RandomAccessFile

- 입출력 클래스 중 유일하게 **파일에 대한 입출력을 동시에 할 수 있는** 클래스
- 파일 포인터로 읽고 쓰는 위치의 이동이 가능
- `RandomAccessFile raf = new RandomAccessFile(파일이름 또는 파일 객체, "rw")`
  - mode가 존재하는데 이는 문서 참조