---
layout: post
title: "SpringBoot_Stack Overflow (순환 참조)"
date: 2024-11-01
categories: blog
category: Error
---

<br>

<details>
<summary>❌ 오류 : StackOverflow</summary>
<h4> DTO와 Entity간 데이터를 변환하는 과정에서 서로의 메서드를 참조하는 오류가 있었다. </h4>

<details>
<summary>Entity</summary>
<div markdown="1">
```java
@Entity
public class Jikwon {
	@Id
	private int jikwonno;
	private String jikwonname;
	private String jikwonjik;
	
	@OneToMany(mappedBy="jikwon", fetch = FetchType.LAZY)
	private List<Gogek> gogekList;
}
```

```java
@Entity
public class Gogek {
	@Id
	private int gogekno;
	private String gogekname;
	private String gogektel;

	@ManyToOne
	@JoinColumn(name="gogekdamsano"/* , referencedColumnName = "jikwonno" */) // 기본 : PK 참조
	private Jikwon jikwon;
}
```
</div>
</details>

<details>
<summary>DTO</summary>
<div markdown="1">

```java
public class JikwonDto {
  private int jikwonno;
	private String jikwonname;
	private String jikwonjik;

	private List<GogekDto> gogekList;

  public static JikwonDto fromEntity(Jikwon entity) {
        return JikwonDto.builder()
        		.jikwonno(entity.getJikwonno())
        		.jikwonname(entity.getJikwonname())
        		.jikwonjik(entity.getJikwonjik())
        		.gogekList(entity.getGogekList()
        				.stream().map(GogekDto::fromEntity)
        				.collect(Collectors.toList()))
        		.build();
    }
}
```

```java
public class GogekDto {
  private int gogekno;
	private String gogekname;
	private String gogektel;
	
	private JikwonDto jikwon;
	
  public static GogekDto fromEntity(Gogek entity) {
        return GogekDto.builder()
                    .gogekno(entity.getGogekno())
                    .gogekname(entity.getGogekname())
                    .gogektel(entity.getGogektel())
                    .jikwon(Jikwon.fromEntity(entity.getJikwon()))
                    .build();
    }
}
```
</div>
</details>

</details>


<details>
<summary>해결 방안</summary>
<li class="font-lg">불필요한 필드 제거</li>
<li class="font-lg">고객과 담당 직원에 대한 정보가 중복되기 때문에</li>
<li class="font-lg">고객 정보 내 직원 필드를 제거하였다.</li>
<div markdown="1">

```java
public class GogekDto {
    private int gogekno;
    private String gogekname;
    private String gogektel;
    
    public static GogekDto fromEntity(Gogek entity) {
        return GogekDto.builder()
                    .gogekno(entity.getGogekno())
                    .gogekname(entity.getGogekname())
                    .gogektel(entity.getGogektel())
                    .build();
    }
}
```

```java
public class JikwonDto {
    private int jikwonno;
    private String jikwonname;
    private String jikwonjik;

    private List<GogekDto> gogekList;

    public static JikwonDto fromEntity(Jikwon entity) {
        return JikwonDto.builder()
                .jikwonno(entity.getJikwonno())
                .jikwonname(entity.getJikwonname())
                .jikwonjik(entity.getJikwonjik())
                .gogekList(entity.getGogekList()
                        .stream().map(GogekDto::fromEntity)
                        .collect(Collectors.toList()))
                .build();
    }
}
```
</div>
<details>