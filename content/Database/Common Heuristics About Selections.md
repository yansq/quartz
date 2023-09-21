
Apply [[Database/Relational Algebra#Selection (σ)|Selections]] as soon as you have the relevant columns:

![[Database/attachments/CleanShot 2023-03-02 at 20.19.59@2x.png]]

The second relation algebra is better than the first one.

---

Keep only the columns you need to evaluate downstream operators

![[Database/attachments/CleanShot 2023-03-02 at 20.41.32@2x.png]]

The second relation algebra is better than the first one.

---

Avoid Cartesian products(笛卡尔积)

Given a choice, do theta-joins rather than cross-products:

Favor `(R ⋈ S) ⋈ T` over `(R X T) ⋈ S`

![[Database/attachments/CleanShot 2023-03-02 at 20.50.47@2x.png]]

