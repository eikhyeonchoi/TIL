# Fixed Length Format
```
전문을 구성하는 field들의 길이를 입력받을 수 있는 최대 사이즈로 고정하는 방법
시스템 간의 통신에서 데이터 송수신 foramt을 정하는 것도 중요(xml, json, fixed lenth format 등)

전문은 일정한 크기의 공통된 데이터를 가진 header 부분과 실제 해당 통신에 필요한 데이터를 가진 body부분 2가지로 나뉜다
전문을 구서앟는 데이터 type에서 int, long 등과 같은 데이터는 시스템 간 endian 문제가 발생할 수 있기에 모두 char로만 구성하는것 이 좋다
※ endian 숫자를 구성하는 바이트를 컴퓨터가 정렬하는방식

예를들어
이름, 전화번호를 보내주는 통신이 있다고 할 때 "이름"은 고정사이즈가 20byte, "전화번호"의 고정사이즈도 20byte라고 한다면
아무개           01012345678
이런 형식으로 "아무개"는 9byte이기에 11byte의 공백문자가 더 들어가야한다
※ UTF-8 한글 3byte/ EUC-KR 한글 2byte
```


```java
// 간단한 자바 구현 예제
public class FixedLengthFormatter {
    // 순서보장을 위해 LinkedHashMap
    private Map<String, byte[]> map = new LinkedHashMap<>();

    // common field
    private String name;
    private String phone;

    // 파라미터를 DTO화 시켜서 타입별로 형변환 시켜 사용하면 편리할 듯
    public FixedLengthFormatter(FixedLengthFormatType type, String name, String phone) {
        this.name = name;
        this.phone = phone;
        setCommonField();
    }

    private void setCommonField() {
        map.put("name", new byte[20]);
        map.put("phone", new byte[20]);
        setZeroPadding(map.get("name"), stringToByte(name, StandardCharsets.UTF_8));
        setZeroPadding(map.get("phone"), stringToByte(phone, StandardCharsets.UTF_8));
    }

    // 오른쪽 정렬, 즉 왼쪽에 제로패딩을 넣는다 가정
    private void setZeroPadding(byte[] total, byte[] partial) {
        int remain = total.length - partial.length;

        for (int i = 0; i < remain; i++) {
            total[i] = '0';
        }

        for (int i = remain, j = 0; i < total.length; i++) {
            total[i] = partial[j++];
        }
    }

    private byte[] stringToByte(String data, Charset charset){
        return data.getBytes(charset);
    }

    public byte[] make() throws IOException {
        ByteArrayOutputStream stream = new ByteArrayOutputStream();
        for(Map.Entry<String, byte[]> e : map.entrySet()) {
            stream.write(e.getValue());
        }

        return stream.toByteArray();
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws IOException {
        FixedLengthFormatter formatter = new FixedLengthFormatter(FixedLengthFormatType.BASIC, "홍길동", "01012345678");
        byte[] make = formatter.make();
        String s = new String(make, StandardCharsets.UTF_8);
        System.out.println("s = " + s);
    }
}

// 결과값
// 00000000000홍길동00000000001012345678
```