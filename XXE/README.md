# XML

## 1. Khái niệm

- *XML (Extensible Markup Language)* là ngôn ngữ đánh dấu mở rộng. Đây là một dạng ngôn ngữ đánh dấu, có chức năng truyền dữ liệu và mô tả nhiều loại dữ liệu khác nhau.

- *XML* không có thẻ riêng, người dùng có thể tạo bất kỳ thẻ nào theo ý muốn ( tuân theo quy tắc của *XML*)

- Các thẻ *XML* khá giống với *HTML*: `tag`, `data`,...

- *XML* được xây dựng theo dạng cây, phải có tối thiểu 1 nút gốc

- *XML* được hỗ trợ phổ biến trên các trình duyệt hiện nay

- Lưu trữ thông tin nhỏ

- Tạo phần tóm tắt nội dung cho website (RSS)

- Tạp sơ đồ cho website ( sitemap )

- Là cầu nối trao đổi dữ liệu giữa các ứng dụng web ( web service )

- ...

- VD:

  ```xml-dtd
  <?xml version="1.0" encoding="UTF-8"?>
  
  <menu>
  	<menu1>
  		<content>Menu1</content>
  		<linkmenu>abc</linkmenu>
  	</menu1>
  	<menu2>
  		<content>Menu2</content>
  		<linkmenu>xyz</linkmenu>
  	</menu2>
  </menu>
  ```

- Vài câu mô tả **XML** hết sức ngắn gọn của *w3s*

  ![xm tutorial](/home/kaito/Pictures/images/Selection_001.png)

---

## 2. XML document structure

### a. XML declarations

- Tất cả tài liệu *xml* cần đều khai báo

  ```xml-dtd
  <?xml
      version="version_number"
      encoding="encoding_declaration"
      standalone="standalone_status" 
  ?>
  ```

  

- VD:

  ```xml-dtd
  <?xml version="1.0" encoding="UTF-8" standalone="no" ?>
  ```

- Trong đó:

  - *version* : chỉ định phiên bản tiêu chuẩn của *XML*
  - *encoding* : chỉ loại mã hóa
  - *standalone* : dùng *yes* nếu sử dụng *internal DTD*, dùng *no* nếu sử dụng *external DTD* 

---

### b. DOCTYPE Declaration & DTDs

- Khai báo loại tài liệu (DOCTYPE) bao gồm *internal DTD* hoặc *external* DTD. Ngoài ra có thể có sự kết hợp của cả *internal* và *external* DTDs

- Internal DTD

  ```xml-dtd
  <!DOCTYPE root_element [
  Document Type Definition (DTD):
    elements/attributes/entities/notations/
    processing instructions/comments/PE references
  ]>
  ```

- VD:

  ```xml-dtd
  <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
  <!DOCTYPE foo [<!ELEMENT foo>]>
  <foo>Hello World.</foo>
  ```

- External DTD:

  - Gồm 2 loại *private* và *public*

- *"Private"* External DTDs:

  ```xml-dtd
  <!DOCTYPE root_element SYSTEM "DTD_location">
  ```

  - Private external DTDs  được định nghĩa bởi từ khóa *SYSTEM*
  - Trong đó *DTD_location* có thể là *relative URL* hoặc *absolute URL*

- VD:

  ```xml-dtd
  <?xml version="1.0" standalone="no" ?>
  <!DOCTYPE document SYSTEM "subjects.dtd">
  
  <document>
    <title>Subjects available in Mechanical Engineering.</title>
    <subjectID>2.303</subjectID>
      <subjectname>Fluid Mechanics</subjectname>
      <prerequisite>
        <subjectID>1.001</subjectID>
        <subjectname>Mathematics</subjectname>
      </prerequisite>
      <classes>4 hours per week (lectures and tutorials) for one
        semester.</classes>
      <assessment>tutorial assignments and one 2hr exam at end of
        course.</assessment>
      <syllabus>
        Fluid statics. The Bernoulli equation. Energy equation. Momentum
        equation. Differential Continuity equation. Differential Energy
        equation. Differential Momentum equation. Dimensional Analysis.
        Similitude. Laminar flow. Turbulent flow. Lift and Drag. Boundary
        layer theory.
      </syllabus>
      <textbooks>
        <author>Foobar</author>
        <booktitle>The Study of Fluid Mechanics</booktitle>
      </textbooks>
  </document>
  ```

- subjects.dtd

  ```xml-dtd
  <!ELEMENT document
    (title*,subjectID,subjectname,prerequisite?,
    classes,assessment,syllabus,textbooks*)>
  <!ELEMENT prerequisite (subjectID,subjectname)>
  <!ELEMENT textbooks (author,booktitle)>
  <!ELEMENT title (#PCDATA)>
  <!ELEMENT subjectID (#PCDATA)>
  <!ELEMENT subjectname (#PCDATA)>
  <!ELEMENT classes (#PCDATA)>
  <!ELEMENT assessment (#PCDATA)>
  <!ATTLIST assessment assessment_type (exam | assignment) #IMPLIED>
  <!ELEMENT syllabus (#PCDATA)>
  <!ELEMENT author (#PCDATA)>
  <!ELEMENT booktitle (#PCDATA)>
  ```

- *"Public"* External DTDs:

  ```xml-dtd
  <!DOCTYPE root_element PUBLIC "DTD_name" "DTD_location">
  ```

  - *"Public"* External DTDs được định nghĩa bởi từ khóa *PUBLIC*
  - *DTD_location* có thể là *relative URL* hoặc *absolute URL* nếu *PUBLIC DTD* không thể xác định được vị trí của *DTD_name*

- *DTD_name* :

  ```xml-dtd
  "prefix//owner_of_the_DTD//description_of_the_DTD//ISO 639_language_identifier"
  ```

- VD:

  ```xml-dtd
  <?xml version="1.0" standalone="no" ?>
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"
    "http://www.w3.org/TR/REC-html40/loose.dtd">
  <HTML>
  <HEAD>
  <TITLE>A typical HTML file</TITLE>
  </HEAD>
  <BODY>
    This is the typical structure of an HTML file. It follows
    the notation of the HTML 4.0 specification, including tags
    that have been deprecated (hence the "transitional" label).
  </BODY>
  </HTML>
  ```

- Combining Internal and External DTD

- VD:

  ```xml-dtd
  <!--inform the XML processor
    that an external DTD is referenced-->
  <?xml version="1.0" standalone="no" ?>
  
  <!--define the location of the
    external DTD using a relative URL address - the open square
    bracket indicates an internal DTD-->
  <!DOCTYPE document SYSTEM "subjects.dtd" [
  
  <!--the markup in the internal DTD
    takes precedence over the external DTD-->
    <!ATTLIST assessment assessment_type (exam | assignment | prac)>
    <!ELEMENT results (#PCDATA)>
  
  <!--close the DOCTYPE declaration-->
  ]>
  ```

- subjects.dtd

  ```xml-dtd
  <!ELEMENT document
    (title*,subjectID,subjectname,prerequisite?,
    classes,assessment,syllabus,textbooks*)>
  <!ELEMENT prerequisite (subjectID,subjectname)>
  <!ELEMENT textbooks (author,booktitle)>
  <!ELEMENT title (#PCDATA)>
  <!ELEMENT subjectID (#PCDATA)>
  <!ELEMENT subjectname (#PCDATA)>
  <!ELEMENT classes (#PCDATA)>
  <!ELEMENT assessment (#PCDATA)>
  <!ATTLIST assessment assessment_type (exam | assignment) #IMPLIED>
  <!ELEMENT syllabus (#PCDATA)>
  <!ELEMENT author (#PCDATA)>
  <!ELEMENT booktitle (#PCDATA)>
  ```

---

### c. Entity

- *Entity* có thể hiểu là 1 biến, phải được khai báo trong DTD và giá trị của nó có thể tham chiếu ở nơi khác trong tài liệu

- Chủ yếu chia thành 4 loại sau:

  - Built-in entities
  - Character entities
  - General entities
  - Parameter entities

- *Entity* có thể chia thành *internal entities* và *external entities*

- Internal entity:

  ```xml-dtd
  <!ENTITY entity name "value of entity" >
  ```

- External entity:

  ```xml-dtd
  <!ENTITY entity name SYSTEM "URI" >
  ```

  
