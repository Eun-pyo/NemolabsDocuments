<span id="main"></span>
# Nemoupload

- [jQuery][] 를 사용하여 간단하게 설정할 수 있습니다.

- HTML 표준 파일 업로드 방식인 [RFC-1867][] 규격으로 업로드 합니다.

- 다수의 파일을 목록에 담아 순차적으로 업로드 합니다.

- 비동기로 업로드 처리를 하며, 서버의 처리 결과를 사용자에게 전달할 수 있습니다.

- 업로드 진행 상태를 확인 할 수 있습니다.

- 파일을 chunk 단위로 나누어 올릴 수 있습니다. 서버에서 지원한다면 이어올리기도 가능합니다.

<span id="ie8-9"></span>
## Internet Explorer 8, 9 지원

Internet Explorer 8, 9 에서는 파일을 업로드하는 기본적인 기능만 사용할 수 있습니다.

다수의 파일을 목록에 담아 비동기로 업로드하며, 서버의 처리 결과를 JSON 형식으로 반환 받습니다.

(_처리 결과를 정상적으로 처리하기 위해서 [서버 설정][]이 필요합니다._)

<span id="ready"></span>
## 준비하기

- **Nemoupload** 는 [jQuery][] 의 플러그인 형태로 되어있습니다.

    ```
    [html]

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    ```

- Nemoupload 파일을 준비합니다.

    ```
    nemoupload
        ├ jQuery.Nemoupload.min.js
        ├ jQuery.Nemoupload.min.css
        └ ./img/
    ```

- 파일과 이미지 디렉토리를 서버에 올립니다.

    _!!! **Nemoupload.css** 와 **./img/** 디렉토리는 같은 경로에 있어야 합니다. !!!_

- HTML 에 스타일과 자바스크립트를 추가합니다.

    ```
    [html]

    <link rel="stylesheet" href="/css/jQuery.Nemoupload.0.0.1.css">
    ```

    ```
    [html]

    <script src="/js/jQuery.Nemoupload.js">
    ```

- 완성된 소스는 다음과 같습니다.

    ```
    [html]

    <!HTML>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>Nemoupload</title>
            <link rel="stylesheet" href="/jQuery.Nemoupload.0.0.1.css">
        </head>
        <body>
            <script src="/jQuery.Nemoupload.0.0.1.js"></script>
        </body>
    </html>
    ```

<span id="start"></span>
## 시작하기

<span id="start-page"></span>
- ### 웹 페이지

    - 업로더가 표시될 div 요소를 작성합니다.
        ```
        [html]

        <div id="nemoupload">
            <p> 자바스크립트가 필요합니다. </p>
        </div>
        ```

    - 옵션값을 넣어 업로더를 생성합니다.
        ```
        [js]

        $('#nemoupload').nemoupload({
            uploadUrl: "/upload"
        });
        ```

    - 완성된 소스는 다음과 같습니다.

        ```
        [html]

        <head>
            <meta charset="UTF-8">
            <title>Nemoupload</title>
            <link rel="stylesheet" href="/jQuery.Nemoupload.0.0.1.css">
        </head>
        <body>
            <div id="nemoupload">
                <p> 자바스크립트가 필요합니다. </p>
            </div>
        ...
            <script src="/jQuery.Nemoupload.0.0.1.js"></script>
            <script>
                $(document).ready(function() {
                    $('#nemoupload').nemoupload({
                        uploadUrl: '/upload'
                    });
                });
            </script>
        </body>
        ...
        ```

<span id="start-server"></span>
- ### 서버 설정

    사용하고 있는 웹 서버가 ` <FORM ENCTYPE="multipart/form-data" ACTION="_URL_" METHOD=POST> ` 파일 업로드를 처리할 수 있다면 그대로 사용할 수 있습니다. (_HTML 표준 파일 업로드 방식인 [RFC-1867][] 규격으로 업로드 합니다._)

    사용환경에 따라서 **chunk를 처리하기 위한 모듈을 사용할 수 있습니다.**  
    Nodejs express: **[document][nodejs-express]**  
    Java Servlet: **[document][java-servlet]**

    Nemoupload 는 파일과 함께 기본 paramter 값을 전달합니다.

    - chunkNumber: chunk 의 번호 입니다. 1 부터 시작합니다. 기본값은 1 입니다.

    - chunkCount: chunk 의 개수 입니다. 1 부터 시작합니다. 기본값은 1 입니다.

    - fileID: 파일의 고유한 ID 입니다. 웹 페이지에서 목록 관리를위해 사용한 **[fileinfo.id][fileinfo-id]** 와 동일합니다.

    chunk 를 사용하게 되면 ` Request header ` 에 ` Nemoupload ` 항목이 담기게 됩니다.

    - Nemoupload.chunkNumber: chunk 의 번호 입니다. 1 부터 시작합니다.

    - Nemoupload.chunkCount: chunk 의 개수 입니다. 1 부터 시작합니다.

    - Nemoupload.chunkSize: chunk 를 나눈 크기 입니다. **[option.chunk][option-chunk]** 값과 동일합니다.

    - Nemoupload.fileID: 파일의 고유한 ID 입니다. 웹 페이지에서 목록 관리를위해 사용한 **[fileinfo.id][fileinfo-id]** 와 동일합니다.

    처리 결과는 JSON 으로 반환해야 합니다.  
    _**[sucess listener][option-listener-success]** 가 호출되는 시점에서 **[fileInfo.response][fileinfo-response]** 에서 결과값을 받을 수 있습니다._  
    _Internet Explorer 8, 9 의 경우 반환되는 ` content-type ` 을 ` text/html ` 로 설정해야 합니다._

<span id="option"></span>
## 옵션

<span id="option-chunk"></span>
- ### chunk

    ` int `  
    파일 하나를 나누어 올립니다.  
    조각 단위는 **byte** 입니다.  
    0 인 경우, 조각 올리기를 사용하지 않습니다.

    ---------------------------------------------------------------------------------

- ### dragAndDrop

    ` boolean `  
    드래그 앤 드롭을 사용하여 파일을 추가할 수 있는 옵션입니다.

    ---------------------------------------------------------------------------------

- ### fieldName

    ` string `  
    업로드 되는 파일의 필드 이름입니다.

    ---------------------------------------------------------------------------------

- ### filter

    ` object `  
    업로드 할 파일의 조건을 설정합니다.

    ---------------------------------------------------------------------------------

    - #### listCount

        ` int `  
        한번에 올릴 수 있는 파일의 개수 입니다.  
        설정하지 않거나 0 이면 제한을 하지 않습니다.

    ---------------------------------------------------------------------------------

    - #### listSize

        ` int `  
        전체 목록의 사이즈를 제한합니다.  
        단위는 **byte** 입니다.  
        설정하지 않거나 0 이면 제한을 하지 않습니다.

    ---------------------------------------------------------------------------------

    - #### maxSize

        ` int `  
        파일 하나의 최대 용량입니다.  
        단위는 **byte** 입니다.  
        설정하지 않거나 0 이면 제한을 하지 않습니다.

    ---------------------------------------------------------------------------------

    - #### minSize

        ` int `  
        파일 하나의 최소 용량입니다.  
        단위는 **byte** 입니다.  
        설정하지 않거나 0 이면 제한을 하지 않습니다.

    ---------------------------------------------------------------------------------

    - #### type

        ` [string] `  
        업로드 할 파일의 확장자를 설정합니다.  
        쉼표로 구분하여 여러개를 설정할 수 있습니다.  
        ex) ['.jpg','.png']

    ---------------------------------------------------------------------------------

- ### listener

    리스너를 등록합니다.

    ---------------------------------------------------------------------------------

    - #### added: function([uploader][], [fileInfo][])

        파일이 추가되면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: [object] `: 추가된 파일들의 정보가 담긴 배열입니다.

    ---------------------------------------------------------------------------------

    - #### cancel: function([uploader][], [fileInfo][])

        업로드 중에, 전송을 취소하면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object`: 전송중이던 파일 정보가 들어있는 객체입니다.

    ---------------------------------------------------------------------------------

    - #### common: function([uploader][], type)

        모든 상황에 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` type: string `: 호출한 리스너 이름입니다.

    ---------------------------------------------------------------------------------

    - #### completed: function([uploader][])

        파일 목록이 전부 처리되면 호출됩니다. 각 항목의 성공여부는 관련 없습니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.

    ---------------------------------------------------------------------------------

    - #### error: function([uploader][], [fileInfo][], error)
        오류가 발생하면 호출됩니다.

        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object`: 오류가 발생한 파일 정보가 들어있는 객체입니다.  
        ` error: {code: 0, message: ''} `: 오류 내용이 담겨있는 객체입니다.

    ---------------------------------------------------------------------------------

    - #### load: function([uploader][])

        업로드 객체가 준비되면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.

    ---------------------------------------------------------------------------------

    - #### progress: function([uploader][], [fileInfo][])

        파일의 데이터가 전송되는 동안 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object`: 전송되고 있는 파일의 정보가 담긴 객체입니다.

    ---------------------------------------------------------------------------------

    - #### remove: function([uploader][], [fileInfo][])

        파일 목록에서 항목을 제거하면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: [object] `: 제거된 파일의 정보들이 담긴 배열입니다.

    ---------------------------------------------------------------------------------

    - #### run: function([uploader][], [fileInfo][])

        파일을 전송하기 시작하면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object`: 전송할 파일 정보가 들어있는 객체입니다.

    ---------------------------------------------------------------------------------

    - #### start: function([uploader][])

        업로드기능을 호출하면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.

    ---------------------------------------------------------------------------------

    <span id="option-listener-success"></span>
    - #### success: function([uploader][], [fileInfo][])

        파일 업로드가 완료 되었을 때 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object `: 성공한 파일의 정보가 들어있는 객체입니다.

    ---------------------------------------------------------------------------------

    - #### warning: function([uploader][], [fileInfo][], error)

        파일 조건이 맞지 않거나, 잘못된 조작을 하면 호출됩니다.  
        ` uploader: object `: 지금 사용되고있는 업로더 객체입니다.  
        ` fileInfo: object`: 전송하던 파일 정보가 들어있는 객체입니다.  
        ` error: {code: 0, message: ''} `: 경고 내용이 담겨있는 객체입니다.

    ---------------------------------------------------------------------------------

- ### nameOverlap: ` true `

    이름이 같은 파일을 목록에 추가할 수 있도록 설정합니다.

    ---------------------------------------------------------------------------------

- ### parameter: ` {field1: v1, field2: v2, ...} `

    파일과 함께 전송할 값들을 설정합니다.

    ---------------------------------------------------------------------------------

- ### showImage: ` true `

    이미지 파일의 미리보기를 설정합니다.  
    파일항목 아이콘 위에 마우스를 올리면 ` width: 150px height:150px ` 크기의 미리보기가 표시됩니다.  
    최대 ` 5MB ` 이하 용량을 가진 이미지만 미리보기가 표시됩니다.

    ---------------------------------------------------------------------------------

- ### titleImg: ` "/img/title.png" `

    타이틀 이미지의 경로를 설정합니다.  
    이미지의 사이즈는 ` width: 432px, height: 57px ` 입니다.

    ---------------------------------------------------------------------------------

- ### uploadUrl ` "/" `

    파일을 업로드 경로를 설정합니다.

    ---------------------------------------------------------------------------------

<span id="nemoupload-object"></span>
## Status

Nemoupload 객체는 플러그인을 생성하는 기본 객체입니다.  
전역에서 접근이 가능합니다.

- Nemoupload.WAITING: 업로드 전, 기본 상태.

- Nemoupload.START: 업로드를 시작.

- Nemoupload.RUN: 전송중.

- Nemoupload.SUCCESS: 업로드 완료.

- Nemoupload.CANCEL: 업로드 취소.

- Nemoupload.FAIL: 업로드 실패.

- Nemoupload.XHR_SUPPORT: XMLHttpReauest 를 사용하여 비동기 업로드가 가능하면 true 를 가집니다.

<span id="uploader-object"></span>
## Uploader

리스너에서 첫번째 인자값으로 **Uploader** 객체를 받습니다.  
**Uploader** 객체는 지금 사용하고 있는 업로드 플러그인을 조작할 수 있는 method 를 제공하고 있습니다.

- ### added([file])

    ` file: [] `  
    브라우저의 File API를 통해 얻어온 File 객체가 담긴 배열을 목록에 추가합니다.  

    인자를 넘기지 않는 경우, 파일 선택창을 열어서 파일을 선택할 수 있게 합니다.  

    ----------------------------------------------------------------

- ### addListener(listener)

    ` listener: {listener: function() {}} `  
    이전에 있던 리스너에 새로운 리스너를 추가합니다.  
    _**!리스너를 초기화 하기 위해서는 [setOptions()][setOptions] 를 사용해야 합니다.**_

    ----------------------------------------------------------------

- ### cancel()

    업로드 중인 작업을 취소합니다.

    ----------------------------------------------------------------

- ### getFileInfo("id") { return [fileInfo][] }

    ` fileInfo: object `  
    ` id ` 를 가지고 파일 정보가 담긴 객체를 반환받는다.

    ----------------------------------------------------------------

- ### getFileInfoArray() { return [fileInfo][] }

    ` fileInfo: [object] `  
    목록에 있는 파일의 정보가 담긴 모든 객체를 반환받는다.

    ----------------------------------------------------------------

- ### getLastActivityFileInfo() { return [fileInfo][] }

    ` fileInfo: object `  
    마지막에 업로드 동작을 했거나, 하고있는 파일의 정보를 반환한다

    ----------------------------------------------------------------

- ### getSize() { return int }

    목록에 있는 파일 전체 용량을 **byte** 값으로 반환한다.

    ----------------------------------------------------------------

- ### getSuccessSize() { return int }

    목록에 있는 파일 중에서 업로드가 성공한 파일 전체 용량을 **byte** 값으로 반환한다.

    ----------------------------------------------------------------

- ### isRun() { return boolean }

    업로드를 하고 있는 중이면 **true**, 그렇지 않으면 **false** 를 반환한다.

    ----------------------------------------------------------------

- ### moveFileIndex(id, index)

    목록에서 ` id: string ` 를 가진 파일을 ` index: int ` 위치로 이동한다.

    ----------------------------------------------------------------

- ### remove(id)

    목록에서 ` id: string ` 를 가진 파일을 지운다.

    ----------------------------------------------------------------

- ### removeAll()

    목록을 전부 지운다.

    ----------------------------------------------------------------

<span id="setOptions"></span>
- ### setOptions(options)

    ` options: object `  
    입력한 옵션값을 새로 정의한다.  
    옵션값을 입력하지 않은 부분은 이전 값을 그대로 반영한다.

    ----------------------------------------------------------------

- ### upload()

    업로드 작업을 시작합니다.

    ----------------------------------------------------------------

<span id="fileinfo-object"></span>
## FileInfo

업로드할 파일의 정보를 가지고 있는 객체 입니다.

- ### chunkCount

    ` int `  
    chunk로 나누었을 경우 생기는 조각의 총 개수 입니다.

    ----------------------------------------------------------------

- ### chunkIndex

    ` int `  
    업로드를 해야하는 조각의 번호 입니다.  
    ` chunkIndex == 3 ` 인 경우, ` chunkIndex <= 2 ` 는 서버에 올라간 상태입니다.

    ----------------------------------------------------------------

- ### chunkSize

    ` int `  
    chunk로 나누었을 경우, chunk 의 크기 입니다.  
    단위는 **byte** 입니다.  
    옵션에 설정한 값과 동일합니다.  
    _File API 를 지원하지 않는 브라우저에서는 지원하지 않습니다._

    ----------------------------------------------------------------

- ### fileObj

    ` object `  
    업로드되는 파일 입니다.  
    File API 에서 제공하는 File 객체가 들어있습니다.  
    _File API 를 지원하지 않는 브라우저에서는 ` <input type="file"/> ` 요소가 들어가 있습니다._

    ----------------------------------------------------------------

<span id="fileinfo-id"></span>
- ### id

    ` string `
    파일을 구별하기위한 고유한 값 입니다.

    ----------------------------------------------------------------

- ### loaded

    ` int `  
    업로드된 용량입니다.  
    단위는 **byte** 입니다.  
    _File API 를 지원하지 않는 브라우저에서는 지원하지 않습니다._

    ----------------------------------------------------------------

- ### name

    ` string `  
    파일의 이름 입니다.

    ----------------------------------------------------------------

- ### percent

    ` int `  
    업로드된 퍼센트를 정수값으로 가지고 있습니다.  
    _File API 를 지원하지 않는 브라우저에서는 지원하지 않습니다._

    ----------------------------------------------------------------

<span id="fileinfo-response"></span>
- ### response

    ` object `  
    업로드 후, 서버에서 반환한 결과값이 들어갑니다.  
    기본값은 ` object ` 입니다.  
    _JSON 을 변환한 객체 입니다. [서버 설정][]이 필요합니다._

    ----------------------------------------------------------------

- ### retryCount

    ` int `
    자동으로 재시도한 횟 수 입니다.  
    3 회를 초과하여 재시도를 하면 오류메세지를 띄우고 0 으로 초기화 됩니다.  
    한 번이라도 성공하면 0 으로 초기화 됩니다.

    ----------------------------------------------------------------

- ### size

    ` int `  
    파일의 사이즈 입니다.  
    단위는 **byte** 입니다.  
    _File API 를 지원하지 않는 브라우저에서는 지원하지 않습니다._

    ----------------------------------------------------------------

- ### speed

    ` int `  
    업로드 된 용량과 걸린 시간을 가지고 계산한 전송 속도 입니다.  
    byte/s 값이 들어갑니다.  
    _**정확한 값이 아닙니다.**_  
    _File API 를 지원하지 않는 브라우저에서는 지원하지 않습니다._

    ----------------------------------------------------------------

- ### status

    ` int `  
    현재 상태값 입니다.  
    [Nemoupload][nemoupload] 객체에 정의된 상태값을 가지고 있습니다.

    ----------------------------------------------------------------

[jQuery]: https://jquery.com/ "제이쿼리"
[nemoupload]: #nemoupload-object
[option]: #option
[option-chunk]: #option-chunk
[option-listener-success]: #option-listener-success
[uploader]: #uploader-object
[fileinfo]: #fileinfo-object
[fileinfo-id]: #fileinfo-id
[fileinfo-response]: #fileinfo-response
[setOptions]: #setOptions
[서버 설정]: #start-server
[nodejs-express]: ServerGuide_NodeJS.html
[java-servlet]: ServerGuide_JavaServlet.html
[RFC-1867]: https://www.ietf.org/rfc/rfc1867.txt
