# Lombok @Builder

对 Lombok 生成的构造器部分重写，实现变量设置默认值，以及变量非空校验：

```java
@Getter
@Builder(builderClassName = "OverwriteBuilder")
public TestPojo {
    private String phoneNumber;

    @Builder.Default
    private Integer amount = 0;

    private String name;

    public static class OverwriteBuilder {
        public OverwriteBuilder amount(Integer amount) {
            this.amount$value = amount;
            this.amount$set = amout != null;
            return this;
        }

        public TestPojo build() {
            if (phoneNumber.isEmpty()) {
                throw new RuntimeException("Invalid phone number");
            }
            return new TestPojo(phoneNumber, amount$value, name);
        }
    }

}
```