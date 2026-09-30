---
aliases:
  - 변환기
tags:
  - pandoc
  - marp
  - 논문
  - 보고서
  - Markdown
  - pptx
  - 발표자료
---
# Pandoc vs Marp  

## 한 줄 요약  

- **Pandoc**: 범용 문서 변환기  

	```zsh
		pandoc 수업자료.md --reference-doc template.pptx -o 01_수업.pptx
	```

- **Marp**: 프레젠테이션 전용 Markdown 도구

```zsh
marp 수업자료.md --pptx

//실시간 미리보기
marp -w lecture.md

```

// marp 기능 및 나의 테마 만들기 찾아보자


---  

## 용도별 비교  

| 항목               | Pandoc       | Marp    |
| ---------------- | ------------ | ------- |
| 목적               | 다양한 문서 포맷 변환 | 슬라이드 제작 |
| Markdown → PPTX  | 가능           | 가능      |
| Markdown → PDF   | 가능           | 가능      |
| Markdown → DOCX  | 가능           | 불가      |
| Markdown → EPUB  | 가능           | 불가      |
| 논문 작성            | 매우 적합        | 부적합     |
| 보고서 작성           | 매우 적합        | 부적합     |
| 발표 자료 작성         | 가능           | 매우 적합   |
| Keynote용 PPTX 생성 | 가능           | 매우 적합   |
| 참고문헌(Citation)   | 지원           | 미지원     |
| LaTeX 수식         | 지원           | 지원      |
| 테마 및 디자인         | 제한적          | 풍부      |
| 학술 활용            | 강점           | 약함      |
| 프레젠테이션 활용        | 보통           | 강점      |


---    

## Pandoc  

### 장점  

- 다양한 문서 형식 지원  
- 논문, 보고서, 책 제작에 강력  
- 참고문헌 및 인용 관리 가능  
- LaTeX 연동 우수  
- 학술 문서 작성에 사실상 표준  
### 단점  

- 슬라이드 디자인 기능이 약함  
- PPTX 레이아웃 커스터마이징이 제한적  
- 발표 자료 제작만을 목적으로 하면 다소 복잡함  

---  

## Marp  

### 장점  

- 슬라이드 제작에 특화  
- Markdown만으로 깔끔한 발표자료 생성  
- 실시간 미리보기 지원  
- PDF, PPTX 출력 품질 우수  
- Keynote 가져오기용 PPTX 생성에 적합  

### 단점  

- 논문, 보고서, EPUB 생성 불가  
- 참고문헌 관리 기능 없음  
- 범용 문서 변환기로 사용하기 어려움  

---  

## 개인 정리  

### Pandoc 사용  

- 논문  
- 연구보고서  
- 학회 원고  
- DOCX/PDF 변환  
- 참고문헌 관리  

### Marp 사용  

- 수업자료  
- 강의 슬라이드  
- 학회 발표자료  
- PPTX 생성 후 Keynote 편집  

---  

