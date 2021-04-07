## Makefileѧϰ�ʼ�
���ߣ�guoqi

### һ�����ļ��

����Ϊguoqiѧϰ����Linuxϵͳ��EDA������д������������Makefile�﷨�ο��ֲᣬ����֮�����½⡣

### ����Makefile����֪ʶ

Makefile��Linuxϵͳ���й��̹����һ�ַ������䶨����Բο�[�ٶȰٿ�](https://baike.baidu.com/item/Makefile/4619787?fr=aladdin)����EDA��Ŀ�����Ĺ�������Ҫ���ϵ��������������ֻ����Ҫ����һ��С��Ŀ���������ô���ķ�ʽҲ����ʵ�֣������Ҫ�Զ�� `.v` �ļ����в�����������Ҫ���ϵ��ظ�ĳЩ��������ô���ķ�ʽЧ�ʷǳ����¡����������Make���̹���ĸ��Makefile��һ���ű��ļ������ǿ��Խ���Ҫִ�е�ָ�����ȱ�д��һ��nameΪMakefile���ļ��У� Ȼ����Linux��Terminal������ `make` �� `make -f Makefile` ���ɡ�

### ����Makefile��д����

#### 1.����1����������һ������Ŀ���ļ�



    target:prerequisites       //Ŀ���������
    <Tab>command               //������ <Tab> ����ͷ�����command��������

������

    all: compile clean simulate

    clean:
        rm -rf csrc DVEfiles simv simv.daidir ucli.key VCS*
    
    Ŀ���all
    �����clean
    ָ  �rm -rf csrc DVEfiles simv simv.daidir ucli.key VCS*

> ˵����Makefile�е�Ŀ����ܻ��кܶ࣬��һ���ļ�ֻ����һ������Ŀ�꣬������allΪ����Ŀ�꣬compile��clean��simulate������������Ŀ�ꡣ

#### 2.����2��ʹ�� `.PHONY` ��һMakefile�ؼ��ֶ���αĿ��

    .PHONY:name          //����һ��αĿ�꣬��Ϊname
    mame:                //����Ҫ����
    <Tab>command1 \      //����ǰ������ <Tab> ����ͷ
    <Tab>command2 \      //�ڽ�β��ͨ���� �ո�+ת�����\�� �������Ӷ���ָ��
    <Tab>command3 

������

    all: compile simulate      //���Ŀ����ԡ� �ո� ���ֿ�
    compile:                   //��VCS����rtl.lst�е��ļ�
        vcs \
        -debug_all \
        -l com.log \
        -f rtl.lst 
    simulate:                  //��VCS����
        ./simv -l sim.log 
    .PHNOY: clean all
        rm -rf csrc DVEfiles simv simv.daidir ucli.key VCS*
    
    αĿ�꣺clean compile simulate
    ָ ��1��(4��)
        vcs \
        -debug_all \
        -l com.log \
        -f rtl.lst	#$(RTL)
    ָ ��2��
        ./simv -l sim.log 
    ָ ��3��
        rm -rf csrc DVEfiles simv simv.daidir ucli.key VCS*

> ˵������Makefile�п����á� �ո�+ת�����\�� �������Ӷ���ָ�Ҫ��ִ��αĿ�꣬����ͨ���� make+αĿ���� ����ʵ�֡��磺�� make clean ����ִ��ɾ��ָ�

#### 3.����3��ʹ�ñ����ḻMakefileָ��

Makefile��������C�����еĺ궨�壬��Ҫ�������֣��û��Զ���������Զ�������Ԥ�������������������

##### 3.1 �û��Զ������

    ���������
    VAR:=value              //������:=����ֵ

    ʹ�ñ�����
    $(������)=???           //��ֵ
    ???=$(������)           //����

������

    RTL:=converter.v       //�������RTL����ʼ��

    compile:               //��VCS����converter.v
        vcs \
        -debug_all \
        -l com.log \
        -f $(RTL)          //���ñ���RTL

##### 3.2 �Զ�����

    $@        //��ǰ�����Ŀ���ļ�
    $<        //��ǰ����ĵ�һ�������ļ�
    $^        //��ǰ��������������ļ�
    $?        //��������������Ŀ���ļ�����������ļ��б����ŷָ�
    $(@D)     //Ŀ���ļ���Ŀ¼�����֣���$@Ϊ�� add/add.v ������$(@D)Ϊadd
    $(@F)     //Ŀ���ļ����ļ������֣���$@Ϊ�� add/add.v ������$(@F)Ϊadd.v

> ˵�����Զ�����������ʹ��ʱ�Զ��滻������ֵ���Զ�����������Ϊ��Makefile���趨�õķ��š���ʹ��EDA���ʱ�������õ��Զ�������

##### 3.3 Ԥ��������ͻ�������

    export    //�鿴Ԥ��������ͻ����������б�

������

    declare -x Laker_HOME="/opt/synopsys/Laker2016"
    declare -x MAIL="/var/spool/mail/crazy"
    declare -x MGC_CALIBRE_LAYOUT_SERVER="127.0.0.1:1989"
    declare -x MGC_HOME="/opt/mentor/calibre2019/aoi_cal_2019.3_25.15"
    declare -x MGC_LIB_PATH="/opt/mentor/calibre2019/aoi_cal_2019.3_25.15/lib"
    declare -x MILKWAY_HOME="/opt/synopsys/milkway_2016"
    declare -x MMSIM_HOME="/opt/cadence/SPECTRE191"
    declare -x Milkyway_HOME="/opt/synopsys/Milkyway_2016"
    declare -x NOVAS_HOME="/opt/synopsys/verdi_2015"
    declare -x OA_UNSUPPORTED_PLAT="linux_rhel50_gcc44x"

> ˵���������߶���ϵͳ���趨�õ��Զ������������һ�㲻��ȥ�޸ģ�Ҳ����ʹ�á�

#### 4.��������

    %.v      //�á� % ����ģʽƥ�䡰 .v ����β���ļ�

������

    compile:converter.v               //��VCS����converter.v
        vcs \
        -debug_all \
        -l com.log \
        -f %.v                        //ģʽƥ��converter.v

> ˵����Makefile����ɷ�Ϊ���ࣺ��ͨ������������ģʽ���������������������ͨ�������������ģʽ������Ҫ���ڱ���C����ʱʹ�ã��ڲ���Linuxϵͳ�е�EDA����ʱʹ�ò���������Ҳ�������ܡ����������Ϊģʽ����֮һ���ʵ��˽⼴�ɡ�

### �ġ�����ע������

1. Makefile��ע��ʹ�á� # ����
2. �����������ʾ���նˣ�������ǰ�ӡ� @ ����
3. ���Makefile���ֲ�Ϊ�� Makefile ���� makefile ������ʹ����� make -f filename ��������make��filenameΪ�������֡�



















