# Transactional(section 2)

<aside>
๐ Transactional์ ์ดํด

</aside>

```java
package hellojpa; 
import javax.persistence.Entity; 
import javax.persistence.Id; 
@Entity 
@Getter
@Setter
public class Member { 
	@Id 
	private Long id; 
	private String name;  
}
```

<aside>
๐ ๊ธฐ๋ณธ ์ ์ธ JPA์ ์คํ ๋์์๋ฆฌ๋ ์๋์ ๊ฐ์ ๊ทธ๋ฆผ์ผ๋ก ์๋๋๋ค. Persistence๋ผ๋ ํด๋์ค์์ ์์์ ํ ํ ์ค์  ์ ๋ณด๋ฅผ ์กฐํ ํ ํ EntityManagerFactory๋ฅผ ์์ฑํํ ๋์์ด ์์๋๋ง๋ค EntityManager๋ฅผ ์์ฑํ๋ค. ๋จผ์  JPA๋ฅผ ์ด์ฉํ์ฌ Entity๋ฅผ ๊ฐ์ ธ์ค๊ฒ ๋๋ฉด Transactional์ ์ปค๋ฐํ๋ ์์ ์ ์ฒดํฌ ํ ํ ๋ณ๊ฒฝ์ ์ด ์๋ค๋ฉด uptade์ปค๋ฐ์ ๋ง๋ค์ด ๋ ๋ฆฌ๊ณ  Transactional์ ์ปค๋ฐํ๋ค.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/593c3afa-470d-4a1a-91e9-fdebc4cd777b/Untitled.png)

# JPA ์๋ ๋ฐฉ์

- [ ]  ์ํฐํฐ ๋งค๋์  ํฉํ ๋ฆฌ๋ฅผ ์์ฑ์์ DB์ฐ๊ฒฐ์ด ๋๋ค.
- [ ]  ์ํฐํฐ ๋งค๋์  ํฉํ ๋ฆฌ๋ ์ดํ๋ฆฌ์ผ์ด์ ๋ก๋ฉ ์์ ์ ํ๋๋ง ์์ฑํด์ผ ํ๋ค.
- [ ]  ์ํฐํฐ ๋งค๋์ ๋ ํ๋์ Transactional(db connection์ด๋ db์ ๋ณ๊ฒฝ์ ์ด ์๊ธธ๋๋ง๋ค)์ ์งํ์์ ํ๋์ฉ ๋ง๋ค์ด์ผ ํ๋ค.
- [ ]  ๊ทธ๋ฆฌ๊ณ  Transactional์ ์คํ ํ ๊ฐ์ ์งํํ๋ค.
- [ ]  ์ ์ผ ์ค์ํ ๊ฒ์ ์ํฐํฐ ๋งค๋์ ๋ ์๋ฐ ์ปฌ๋ ์์ผ๋ก ์๊ฐํ๋ฉด ๋๋ค. ๋ด ์๋ฐ ์ปฌ๋ ์์ ๋์  ์ ์ฅํด์ฃผ๋ ์ญํ์ ํด์ค๋ค.

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

//        DB์ ์ปค๋ฅ์์ ํ๋ ์ป์ ๊ฒ์ด๋ค.
        EntityManager em = emf.createEntityManager();
//        Transaction ์์
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        Member member = new Member();
        member.setId(1L);
        member.setName("helloA");
//      JPA์ ์ฅ
        em.persist(member);
//      commit
        tx.commit();

        em.close();
        emf.close();
    }
}
```

<aside>
๐ก ์ถ๋ ฅ๊ฐ์ ์๋์ ๊ฐ์ผ๋ฉฐ DB๋ฅผ ์กฐํ ํ๊ฒ ๋๋ฉด ์๋์ ๊ฐ์ด ์ฟผ๋ฆฌ๋ฌธ์ ์์ฑํ์ง ์์์์๋ ์ค์ ๋ก ๋ฐ์ดํฐ๋ค์ด DB์ ์ ์ฅ๋๊ฒ ๋๋ค.

</aside>

```python
<property name="hibernate.show_sql" value="true"/> : ์ฟผ๋ฆฌ์ ๊ฐ์ด ์ค์ ๋ก ์ถ๋ ฅ์ด ๋๊ฒ ํ๋ ๊ตฌ๋ฌธ
<property name="hibernate.format_sql" value="true"/> : ์ฟผ๋ฆฌ๋ฅผ ๋ณด๊ธฐ ์ฝ๊ฒ ํฌ๋ฉงํํ ๊ตฌ๋ฌธ
<property name="hibernate.use_sql_comments" value="true"/> : ์ฟผ๋ฆฌ๊ฐ ๋์จ ์ด์ ๋ฅผ ์๋ ค์ฃผ๋ ๊ตฌ๋ฌธ ์ฃผ์ ์ฒ๋ฆฌ ๋ ๋ถ๋ถ
Hibernate: 
    /* insert hellojpa.Member
        */ insert 
        into
            Member
            (name, id) 
        values
            (?, ?)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0542658-07af-44cb-a45b-67ffd1b3fed8/Untitled.png)

---

### ํ์ง๋ง ์ด๋ด ๊ฒฝ์ฐ ์์ธ ์ํฉ์ ๋์ฒ ํ  ์ ์๊ธฐ์ ์๋์ ๊ฐ์ ์ฝ๋๋ก ๊ตฌํํ๋ ๊ฒ์ด ์ ์์ด๋ค.

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

//        DB์ ์ปค๋ฅ์์ ํ๋ ์ป์ ๊ฒ์ด๋ค.
        EntityManager em = emf.createEntityManager();
//        Transaction ์์
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        try {
            Member member = new Member();
            member.setId(2L);
            member.setName("helloB");
//      JPA์ ์ฅ
            em.persist(member);
//      commit
            tx.commit();
        } catch (Exception e){
            tx.rollback();
        } finally {
//            EntityManager๊ฐ DB์ปค๋ฅ์์ ๋ฌผ๊ณ  ๋์ํ๊ธฐ์ ์ฌ์ฉ์ ํ๊ณ  ๋ซ์์ค์ผ ํ๋ค.
            em.close();
        }
        emf.close();
    }
}
```

- ํ์ ์์ 
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB์ ์ปค๋ฅ์์ ํ๋ ์ป์ ๊ฒ์ด๋ค.
            EntityManager em = emf.createEntityManager();
    //        Transaction ์์
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                System.out.println(findMember.getId());
                System.out.println(findMember.getName());
    //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager๊ฐ DB์ปค๋ฅ์์ ๋ฌผ๊ณ  ๋์ํ๊ธฐ์ ์ฌ์ฉ์ ํ๊ณ  ๋ซ์์ค์ผ ํ๋ค.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
    <aside>
    ๐ก ์ถ๋ ฅ๊ฐ : ์กฐํ์์ select์ฟผ๋ฆฌ๊ฐ ์๋์ ์ผ๋ก ๋ ์๊ฐ๋ฉฐ ๊ฐ์ ๋ค๊ณ  ์ค๋ ๊ฒ์ ํ์ธ ํ  ์ ์๋ค.
    
    </aside>
    
    ```java
    Hibernate: 
        select
            member0_.id as id1_0_0_,
            member0_.name as name2_0_0_ 
        from
            Member member0_ 
        where
            member0_.id=?
    1
    helloA
    ```
    
- ํ์ ์ญ์ 
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB์ ์ปค๋ฅ์์ ํ๋ ์ป์ ๊ฒ์ด๋ค.
            EntityManager em = emf.createEntityManager();
    //        Transaction ์์
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                
    						em.remove(findMember);
    //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager๊ฐ DB์ปค๋ฅ์์ ๋ฌผ๊ณ  ๋์ํ๊ธฐ์ ์ฌ์ฉ์ ํ๊ณ  ๋ซ์์ค์ผ ํ๋ค.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
- ํ์ ์์ 
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB์ ์ปค๋ฅ์์ ํ๋ ์ป์ ๊ฒ์ด๋ค.
            EntityManager em = emf.createEntityManager();
    //        Transaction ์์
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                findMember.setName("helloJPA");
                
                //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager๊ฐ DB์ปค๋ฅ์์ ๋ฌผ๊ณ  ๋์ํ๊ธฐ์ ์ฌ์ฉ์ ํ๊ณ  ๋ซ์์ค์ผ ํ๋ค.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
    <aside>
    ๐ก ์ถ๋ ฅ ๊ฐ์ ์๋์ ๊ฐ์ด ๋์ค๋ฉฐ ์ปฌ๋ ์์ฒ๋ผ ํ์ฉ์ ํ๊ธฐ์ ์ ์ฅํ๋ ๋ฉ์๋๋ ํ์๊ฐ ์๊ฒ ๋๋ค. JPA๋ฅผ ํตํด Entity๋ฅผ ๋ค๊ณ  ์ค๊ฒ ๋๋ฉด JPA๊ฐ Transactional์ ์ปค๋ฐํ๋ ์์ ์ ๋ณ๊ฒฝ ์ฌํญ์ด ์๋์ง ์ฒดํฌํ๊ฒ ๋๋ค. ๋ณ๊ฒฝ์ ์ด ์์ ์์ ์๋์ ์ผ๋ก ์ปค๋ฐํ๊ธฐ ์ง์ ์ UPDATE์ฟผ๋ฆฌ๋ฅผ ๋ง๋ค์ด์ ์ฌ์ฉํ๋ค.
    
    </aside>
    
    ```java
    Hibernate: 
        select
            member0_.id as id1_0_0_,
            member0_.name as name2_0_0_ 
        from
            Member member0_ 
        where
            member0_.id=?
    Hibernate: 
        /* update
            hellojpa.Member */ update
                Member 
            set
                name=? 
            where
                id=?
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e82ad421-2e40-48dd-9b95-fa726090fb2d/Untitled.png)
    

## ์ฃผ์ ์ฌํญ

1. ์น ์๋ฒ๊ฐ ์ฌ๋ผ์ค๋ ์์  DB๋น *`EntityManagerFactory`*  ๋ ํ๋๋ง ์์ฑ์ด ๋๋ค.
2. *`EntityManager`* ๋ ๊ณ ๊ฐ์ ์์ฒญ์ด ์ฌ ๋๋ง๋ค ์์ฑ ๋์๋ค๊ฐ ์ญ์ ๋๋ ๋ฐฉ์์ ์ฌ์ฉํ๋ค.
3. *`EntityManager`* ๋ ์ฐ๋ ๋๋ง๋ค ๊ณต์  ํ  ์ ์๊ณ  ์ฌ์ฉํ๊ณ  ๋ฒ๋ ค์ผ ํ๋ค.
4. JPA์ ๋ชจ๋  ๋ฐ์ดํฐ์ ๋ณ๊ฒฝ์ *`Transaction`* ์์์๋ง ์คํ๋๋ค.

<aside>
๐ก JPQL : ๋ง์ Table์ค์์ ๋ด๊ฐ ์ํ๋ ๋ฐ์ดํฐ๋ค์ ์ด๋ป๊ฒ ์กฐํ ํ  ๊ฒ์ธ์ง๋ฅผ ํด๊ฒฐํ๊ฒ ํด์ฃผ๋ ๋ฐฉ์

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc4bfbb4-44f0-4c42-bfa6-18da0e24655e/Untitled.png)

1. ๊ฒ์ ์ฟผ๋ฆฌ์ ๊ฒฝ์ฐ ๋ฐ์ดํฐ๋ฅผ ๊ฒ์์ ํด์ ๋ค๊ณ  ์์ผ ํ๋ค. ํ์ง๋ง ํ์ด๋ธ์ ๊ฒ์์์ ๋ฌธ๋ฒ์ด ์ด๊ธ๋๋ฏ๋ก ์ํฐํฐ ๊ฐ์ฒด๋ฅผ ๋์์ผ๋ก ๊ฒ์์ ํด์ผ ํ๋ค.
2. ๊ฒ์ ์กฐ๊ฑด์ด ํฌํจ๋ SQL์ด ํ์ํ๋ฐ ์ค์  ๋ฌผ๋ฆฌ์ ์ธ ํ์ด๋ธ์ธ RDB์ ์ฟผ๋ฆฌ๋ฅผ ๋ ๋ฆฐ๋ค๋ฉด ํด๋น ๋๋น์ ์ข์์ ์ธ ์ค๊ณ๊ฐ ๋๋ฒ๋ฆฐ๋ค. ๊ทธ๋ ๊ธฐ์ ์ํฐํฐ ๊ฐ์ฒด๋ฅผ ๋์์ผ๋ก ์ฌ์ฉํ  ์ ์๋ ์ฟผ๋ฆฌ๊ฐ ์ ๊ณต๋๋ค.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66932ce9-a52f-4abf-901c-31c5ed844229/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c83f0cc7-c06b-4abe-9448-d43ab49e515c/Untitled.png)

<aside>
๐ก ์คํ์์ ํ์ฌ ๋ฐ์ดํฐ์ ๋ง๋ ์ ์ ํ ์ฟผ๋ฆฌ๋ฌธ์ด ์์ฑ๋๋ค.

</aside>

## ๐๏ธํต์ฌ ๋ด์ฉ

1. ๋ชจ๋  ๋ณ๊ฒฝ์ *`Transaction`* ์์์ ์คํ๋๋ฉฐ ์กฐํ ๊ฐ์ ๋ฐ์ดํฐ์ ๊ฐ์ ๋ณ๊ฒฝํ์ง ์๋ ๋ฉ์๋๋ *`Transaction`* ์ ์ ์ธํ์ง ์์๋ ์ ์๋ํ๋ค.
2. *`Transaction`* commit์ ํ์ง ์๋๋ค๋ฉด ๋ฐ์์ด ๋์ง ์๋๋ค.
3. *`EntityManager`* ๋ ํ๋์ ๋์์ด ๋๋ ์ ๋ซ๊ณ  ๋ค๋ฅธ ๋์์ด ๋ฐ์์ ์๋์ ์ผ๋ก ์์ฑ๋๋ค.
4. WAS๊ฐ ๋ด๋ ค๊ฐ ๊ฒฝ์ฐ *`ManagerFactory`* ๋ฅผ ๋ซ์์ค์ผ๋ง ์ปค๋ฅ์ํ๋ง ๊ฐ์ ๋ถ๋ถ์ ๋ฆฌ์์ค๊ฐ ํด์ฒด๋๊ฒ ๋๋ค.
