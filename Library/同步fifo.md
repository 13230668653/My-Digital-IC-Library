
## ͬ��FIFO

### 1����ظ���

fifo�� first input first output ����д�����Ƚ��ȳ����У�fifoһ��������ͬʱ����Ļ�������fifo���ݶ���д��ʱ���Ƿ�Ϊͬһʱ�Ӷ���Ϊͬ��fifo���첽fifo���첽fifo���ͬ��fifo��˵����Ƹ��Ӹ���һ�㡣

���FIFO��ʱ��һ����Ҫ�������㣺

**1. FIFO�Ĵ�С**

FIFO�Ĵ�Сָ����ram�Ĵ�С��������Ը��������Ҫ�����á�

**2. FIFO����״̬���ж�**

FIFO����״̬���ж�ͨ�������ַ�����

a���첽FIFO�е�ramһ����˫�˿�ram�������ж����Ķ�д��ַ����˿���һ���Ƕ�ָ�룬һ����дָ�룬��ָ��ָ����һ��Ҫ���ĵ�ַ��дָ��ָ����һ��Ҫд�����ݵĵ�ַ�����ͨ���Ƚ϶�ָ���дָ��Ĵ�С��ȷ������״̬��

b������һ������������дʹ����Ч��ʱ���������һ������ʹ����Ч��ʱ�򣬼�������һ������������ram��size���бȽ����ж�fifo�Ŀ���״̬�����ַ�����ƱȽϼ򵥣�������Ҫ�Ķ���ļ��������ͻ�����������Դ�����ҵ�fifo�Ƚϴ�ʱ���ή��fifo���տ��Դﵽ���ٶȡ�

����Ŀ�е�FIFOΪͬ��FIFO������b�����жϿ�����

### 2����ƴ���

    /******************************************************
    �ο��ԣ�https://blog.csdn.net/buzhiquxiang/article/details/103287220
    ����ߣ�����
    fifo���ͣ�ͬ��fifo
    �ص㣺1����ʱ����дʱ��Ϊͬһʱ�ӣ���ʱ�������ز�ͬʱ������д��������д˳��Ϊ����д�������
          2��fifo�ڱ�д������Լ���д�룬�Ӷ�����ԭ�����ݣ���д��ʱ����д�����棻
          3��fifo��ȡ��ɺ�ɼ������ж��������ڶ�ȡ���ʱ�������꾯�棬��ȡ��ͷ��ʼ��
          4��full=1��ʾд����full=0��ʾд�룬empty=1��ʾ��ȡ��empty=0��ʾ���ꣻ
    BUG��1������0���ж�Ϊ�գ���δ�޸���
    *******************************************************/
    
    module fifo_s #(parameter WIDTH=8,DEPTH=8,ADDR=3)    //���Ϊ8,���Ϊ    2^7=128 
    (clk,reset_n,wen,ren,data_in,full,empty,data_out);
    
    input wire clk,reset_n;
    input wire wen,ren;                    //дʹ�ܣ���ʹ��
    input wire [WIDTH-1:0] data_in;
    output reg full,empty;                 //������
    output reg [WIDTH-1:0] data_out; 
    
    reg [WIDTH-1:0] memery [DEPTH-1:0];    //�ڴ�����Ϊ:8
    reg [ADDR-1:0] waddr,raddr;            //д��ַָ�룬����ַָ��
    
    always@(posedge clk,negedge reset_n) begin   
    if(reset_n == 0) waddr = 0;
    else if(wen == 1) begin
        if((data_in != 0)&&(full != 1)) begin
            memery[waddr] = data_in;                  //д�Ĵ���
    		waddr = waddr + 1;
    	end
        else waddr = waddr;
    end
    else waddr = waddr;
    end
    
    always@(posedge clk,negedge reset_n) begin    
    if(reset_n == 0) raddr = 0;
    else if(ren == 1) begin
        if((memery[raddr] != 0)&&(empty != 0)) begin
          data_out = memery[raddr];                //���Ĵ���
    		raddr = raddr + 1;
        end
        else raddr = raddr;
    end
    else raddr = raddr;
    end
    
    always@(posedge clk,negedge reset_n) begin    //�ж��Ƿ�д��
    if(reset_n == 0) full = 0;
    else if(waddr == DEPTH) begin
        full = 1;
        waddr = 0;
    end
    else full = 0;
    end
    
    always@(posedge clk,negedge reset_n) begin    //�ж��Ƿ����
    if(reset_n == 0) empty = 1;
    else if(raddr==DEPTH) begin
        empty = 0;
        raddr = 0;
    end
    else empty = 1;
    end
    
    endmodule

### 3�����Դ���

    `timescale 1ns/1ns
    module fifo_s_t();
    
    parameter WIDTH=8,DEPTH=8,ADDR=4;
    reg clk,reset_n,wen,ren;
    reg [WIDTH-1:0] data_in;
    wire full,empty;
    wire [WIDTH-1:0] data_out;
    
    fifo_s #(WIDTH,DEPTH,ADDR) U1 (clk,reset_n,wen,ren,data_in,full,empty,    data_out);
    
    always #5 clk=~clk;
    
    initial begin
    clk = 0;
    reset_n = 0;
    wen = 0;
    ren = 0;
    #18
    reset_n = 1;
    wen = 1;
    ren = 0;
    #100
    wen = 0;
    ren = 1;
    #182
    reset_n = 0;
    end
    
    initial begin
    #20 data_in = 10;
    #10 data_in = 5;
    #10 data_in = 6;
    #10 data_in = 7;
    #10 data_in = 89;
    #10 data_in = 125;
    #10 data_in = 3;
    #10 data_in = 9;
    #10 data_in = 22;
    #10 data_in = 23;
    #10 data_in = 220;
    #10 data_in = 96;
    #10 data_in = 155;
    #10 data_in = 53;
    #10 data_in = 62;
    #10 data_in = 29;
    #10 data_in = 85;
    #10 data_in = 44;
    #10 data_in = 11;
    #10 data_in = 3;
    end
    
    endmodule

### 4��Modelsim����

![fifo_s_����.png](../Picture/fifo_s_����.png)

### 5��RTLͼ

![fifo_s_����.png](../Picture/fifo_s_����.png)

�ο����ף�

1. [ͬ��FIFO���첽FIFO](https://blog.csdn.net/buzhiquxiang/article/details/103287220)

