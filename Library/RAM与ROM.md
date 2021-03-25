## RAM��ROM

**���ߣ�����**

----------------------------------

## 1��RAM��������ʴ洢����
����RAM��ֻ��һ���������ߡ���ַ���ߺͶ�д�����ߣ���˵����������Ҫ����ͬһ�鵥��RAM ʱ����Ҫͨ���ٲõ�·���жϡ�

˫��RAM������������ȫ�����������ߡ���ַ�ߺͶ�д�����ߣ��Ӷ�ʵ���˴������ݵĸ��ٷ����Լ���ͬʱ��������ݽ�����

### 1.1 ����RAM

˵����

1. cs_nΪƬѡ�źţ�����Ч����cs_nΪ��ʱ���洢�����ڹ���״̬�����Զ���д������cs_nΪ��ʱ���洢�����ڽ�ֹ״̬��ǿ�����0����

2. we_nΪдʹ���źţ�����Ч����we_nΪ��ʱ���洢������д״̬����we_nΪ��ʱ���洢�����ڶ�״̬��

3. ram������Ϊ8*8=64bit���ݡ�

��ƴ��룺

    /**********����RAM***************
    ����ߣ�xygq163
    ˵����1��cs_nΪƬѡ�źţ�����Ч����cs_nΪ��ʱ���洢�����ڹ���״̬�����Զ���д��    ����cs_nΪ��ʱ���洢�����ڽ�ֹ״̬��ǿ�����0����
          2��we_nΪдʹ���źţ�����Ч����we_nΪ��ʱ���洢������д״̬����we_nΪ��    ʱ���洢�����ڶ�״̬��
          3��ram������Ϊ8*8=64bit���ݡ�
    *********************************/
    module ram_single #(parameter WIDTH=8,DEPTH=8)
    (clk,reset_n,cs_n,we_n,addr,data_in,data_out);
    
    input wire clk,reset_n,cs_n,we_n;
    input wire [2:0] addr; 
    input wire [WIDTH-1:0] data_in;
    output reg [WIDTH-1:0] data_out;
    
    reg [WIDTH-1:0] ram [DEPTH-1:0];    //������һ���洢��
    integer i;
    
    always@(posedge clk,negedge reset_n)
     begin
        if(!reset_n) begin
    	    data_out <= 0;
    	    for(i=0;i < WIDTH;i=i+1)
    	        ram[i] <= 0;
    	end
        else if(!cs_n) begin
            if(!we_n) ram[addr] <= data_in;
            else data_out <= ram[addr];		
    	end
        else begin
    	    data_out <= 0;
    	end
     end
    
    endmodule

���Դ��룺

    `timescale 1ns/1ns
    module ram_single_t();
    
    parameter WIDTH=8,DEPTH=8;
    
    reg clk,reset_n,cs_n,we_n;
    reg [2:0] addr;
    reg [WIDTH-1:0] data_in;
    wire [WIDTH-1:0] data_out;
    
    ram_single #(WIDTH,DEPTH) U1(clk,reset_n,cs_n,we_n,addr,data_in,data_out);
    
    always #5 clk=~clk;
    
    initial begin
    clk = 0;
    reset_n = 0;
    cs_n = 0;
    we_n = 0;
    addr = 0;
    data_in = 0;
    end
    
    initial begin
    #10 reset_n = 1;
    #10 data_in = 5;
    addr = addr + 1;
    #10 data_in = 12;
    addr = addr + 1;
    #10 data_in = 36;
    addr = addr + 1;
    #30 we_n = 1;
    #10 addr = addr - 1;
    #10 addr = addr - 1;
    #10 addr = addr - 1;
    #20 cs_n = 1;
    end
    
    endmodule

Multisim���棺

![ram_single���ܷ���.png](../Picture/)

### 1.2 ˫��RAM

˵����

1. cs_nΪƬѡ�źţ�����Ч����cs_nΪ��ʱ���洢�����ڹ���״̬�����Զ���д������cs_nΪ��ʱ���洢�����ڽ�ֹ״̬��ǿ�����0����

2. we_nΪдʹ���źţ�����Ч����we_nΪ��ʱ���洢������д״̬����we_nΪ��ʱ���洢�����ڶ�״̬��

3. ram������Ϊ8*8=64bit���ݡ�

4. addr_r��ʾ����ַ����clk_2���ƣ�addr_w��ʾд��ַ����clk_1���ơ�

��ƴ��룺

    /**********˫��RAM****************
    ����ߣ�xygq163
    ˵����1��cs_nΪƬѡ�źţ�����Ч����cs_nΪ��ʱ���洢�����ڹ���״̬�����Զ���д)����cs_nΪ��ʱ���洢�����ڽ�ֹ״̬��ǿ�����0����
          2��we_nΪдʹ���źţ�����Ч����we_nΪ��ʱ���洢������д״̬����we_nΪ��ʱ���洢�����ڶ�״̬��
          3��ram������Ϊ8*8=64bit���ݡ�
          4��addr_r��ʾ����ַ����clk_2���ƣ�addr_w��ʾд��ַ����clk_1���ơ�
    *******************************/
    module ram_double #(parameter WIDTH=8,DEPTH=8,NUMBER=3)
    (clk_1,clk_2,reset_n,cs_n,we_n,addr_r,addr_w,data_in,data_out);
    
    input wire clk_1,clk_2,reset_n,cs_n,we_n;
    input wire [NUMBER-1:0] addr_r,addr_w;
    input wire [WIDTH-1:0] data_in;
    output reg [WIDTH-1:0] data_out;
    
    reg [WIDTH-1:0] ram [DEPTH-1:0];    //������һ���洢��
    integer i,j;
    
    always@(posedge clk_1,negedge reset_n)
     begin
        if(!reset_n) begin
    	    data_out <= 0;
    	    for(i=0;i < WIDTH;i=i+1)
    	        ram[i] <= 0;		
        end
    	else if(!cs_n) begin
    	    if(!we_n) ram[addr_w] <= data_in;
    	    else ;
    	end
    	else begin
            data_out <= 0;
    	end
     end
     
    always@(posedge clk_2,negedge reset_n)
     begin
        if(!reset_n) begin
    	    data_out <= 0;
    	    for(j=0;j < WIDTH;j=j+1)
    	        ram[j] <= 0;		
        end
    	else if(!cs_n) begin
    	    if(we_n) data_out <= ram[addr_r];
    	    else ;
    	end
    	else begin
            data_out <= 0;
    	end
     end
    
    endmodule

���Դ��룺

    `timescale 1ns/1ns
    module ram_double_t();
    
    parameter WIDTH=8,DEPTH=8,NUMBER=3;
    
    reg clk_1,clk_2,reset_n,cs_n,we_n;
    reg [NUMBER-1:0] addr_r,addr_w;
    reg [WIDTH-1:0] data_in;
    wire [WIDTH-1:0] data_out;
    
    ram_double #(WIDTH,DEPTH,NUMBER) U1(clk_1,clk_2,reset_n,cs_n,we_n,addr_r,addr_w,data_in,data_out);
    
    always #5 clk_1=~clk_1;
    always #10 clk_2=~clk_2;
    
    initial begin
    clk_1 = 0;
    clk_2 = 0;
    reset_n = 0;
    cs_n = 0;
    we_n = 0;
    data_in = 0;
    addr_r = 0;
    addr_w = 0;
    end
    
    initial begin
    #10 reset_n = 1;
    #10 data_in = 5;
    #1 addr_w = addr_w + 1;
    #10 data_in = 12;
    #1 addr_w = addr_w + 1;
    #10 data_in = 36;
    #1 addr_w = addr_w + 1;
    #30 we_n = 1;
    #10 addr_r = addr_r + 1;
    #10 addr_r = addr_r + 1;
    #10 addr_r = addr_r + 1;
    #20 cs_n = 1;
    end
    
    endmodule

Multisim���棺

![ram_double���ܷ���.png](../Picture/)

## 2��ROM��ֻ���洢����

˵����

1. ��������߼���ƣ���������ʱд�����ݣ��Ժ�ֻ�ܶ�������д�룩��
2. read=1ʱ�����ݣ�read=0ʱ�������̬������8'bz��ͬ��8'bzzzzzzzz��

��ƴ��룺

    /***********ROM�����*********************
    ����ߣ�xygq163
    ˵����1����������߼���ƣ���������ʱд�����ݣ��Ժ�ֻ�ܶ�������д�룩��
          2��read=1ʱ�����ݣ�read=0ʱ�������̬������8'bz��ͬ��8'bzzzzzzzz��
    **************************************/
    module rom #(parameter WIDTH=8,DEPTH=8,NUMBER=3)
    (read,addr,data_out);
    
    input wire read;
    input wire [NUMBER-1:0] addr;
    output wire [WIDTH-1:0] data_out;
    
    wire [WIDTH-1:0] rom [DEPTH-1:0];    //������һ���洢����wire���ͣ�
    
    assign rom[0] = 1;
    assign rom[1] = 2;
    assign rom[2] = 3;
    assign rom[3] = 4;
    assign rom[4] = 5;
    assign rom[5] = 6;
    assign rom[6] = 7;
    assign rom[7] = 8;
    
    assign data_out = read ? rom[addr]:8'bz;
    
    endmodule

���Դ��룺

    `timescale 1ns/1ns
    module rom_t();
    
    parameter WIDTH=8,DEPTH=8,NUMBER=3;
    
    reg read;
    reg [NUMBER-1:0] addr;
    wire [WIDTH-1:0] data_out;
    
    rom #(WIDTH,DEPTH,NUMBER) U1(read,addr,data_out);
    
    initial begin
    read = 0;
    addr = 0;
    end
    
    initial begin
    #10 read = 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    #10 addr = addr + 1;
    end
    
    endmodule

Multisim���棺

![rom���ܷ���.png](../Picture/)

**�ο����ϣ�**
1. [RAM/ROM�洢�������](https://blog.csdn.net/Andy_ICer/article/details/105371780)
2. [verilog case��if��������ȫ����������������](https://blog.csdn.net/qq_40696831/article/details/88855164)

