# MyBlogMetadate

- 개인 블로그

## Contributor

@9utty(이건학)

## Deploy

- AWS Amplify

## script

```sh
npm install
```

```sh
npm run dev
```

## 테이블 생성

```sql
-- blog_main_menu 테이블 생성
CREATE TABLE blog_main_menu (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- blog_sub_menu 테이블 생성
CREATE TABLE blog_sub_menu (
    id SERIAL PRIMARY KEY,
    main_menu_id INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (main_menu_id) REFERENCES blog_main_menu(id) ON DELETE CASCADE
);

-- blog_items 테이블 생성
CREATE TABLE blog_items (
    id SERIAL PRIMARY KEY,
    main_menu_id INT,
    sub_menu_id INT,
    title VARCHAR(255) NOT NULL,
    path VARCHAR(255) NOT NULL,
    view_count INT DEFAULT 0,
    tag VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (main_menu_id) REFERENCES blog_main_menu(id) ON DELETE CASCADE,
    FOREIGN KEY (sub_menu_id) REFERENCES blog_sub_menu(id) ON DELETE CASCADE
);
```

## 테이블 삭제

```sql
DROP TABLE blog_main_menu;
DROP TABLE blog_sub_menu;
DROP TABLE blog_items;
```

## Main menu insert

```sql
INSERT INTO blog_main_menu (name) VALUES ('Javascript')
```

## Main menu delete

```sql
DELETE FROM blog_main_menu
WHERE name='Javascript';
```

## Sub menu insert

```sql
INSERT INTO blog_sub_menu (name, main_menu_id)
VALUES ('Javascript', 1);
```

## item 등록 쿼리 예시

```sql
INSERT INTO blog_items (main_menu_id, sub_menu_id, title, path, tag)
VALUES (3, 7, 'sitemap에 대해서', 'Blog/Sitemap.mdx', 'frontend, sitemap');
```

## 메뉴 가져오기 쿼리 예시

```sql
SELECT 
    mm.id AS main_menu_id,
    mm.name AS main_menu_name,
    sm.id AS sub_menu_id,
    sm.name AS sub_menu_name,
    bi.id AS blog_item_id,
    bi.title AS blog_item_title,
    bi.content AS blog_item_content
FROM 
    blog_main_menu mm
LEFT JOIN 
    blog_sub_menu sm ON mm.id = sm.main_menu_id
LEFT JOIN 
    blog_items bi ON mm.id = bi.main_menu_id OR sm.id = bi.sub_menu_id
ORDER BY 
    mm.id, sm.id, bi.id;
```