## 内存视角：C语言控制硬件的本质是操作内存

![](images/b80d6ae206c353e006fe839239fd38e04bd5b802b9a50999821813ba99bf859a.jpg)

<details>
<summary>flowchart</summary>

```mermaid
graph LR
  subgraph CPU
    CPU1["CPU"]
    CPU2["地址信息"]
    CPU3["控制信息"]
    CPU4["寄存器"]
  end

  subgraph Storage
    Storage1["内存"]
    Storage2["EF"]
    Storage3["1E"]
    Storage4["23"]
    Storage5["46"]
    Storage6["......"]
  end

  subgraph External
    External["外设"]
    External1["GPIO"]
    External2["I2C"]
    External3["UART"]
    External4["......"]
  end

  subgraph Devices
    Device1["LED"]
    Device2["传感器"]
    Device3["屏幕"]
  end

  CPU1 -.->|地址总线| Storage1
  CPU2 -.->|控制总线| Storage1
  CPU3 -.->|数据总线| Storage1
  CPU4 -.->|数据总线| Storage1

  Storage1 -->|地址编号| External
  Storage2 -->|地址编号| External
  Storage3 -->|地址编号| External
  Storage4 -->|地址编号| External
  Storage6 -->|地址编号| External
  External -->|控制映射| Storage1
  External -->|控制映射| Storage2
  External -->|控制映射| Storage3
  External -->|控制映射| Storage4
```
</details>