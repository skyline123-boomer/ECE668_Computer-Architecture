# ECE668_TomasuloAlgorithm
Platform: Visual Studio 2022
Program Language: C++
___

###This program start at 2021 fall###
1. Computer Architecture : A Quantitative Approach (5th Edition) by Hennessy, John L., Patterson, David A.
2. Instructor : Yadi Eslami
3. Teammember：Junfei Wang, Jianyun Mu, Nengxing Shen

___

#INSTRUCTIONS FOR USE OF THIS PROGRAM:
#**1. SETUP RESERVATION STATION ARCHITECTURE:**

# a. Define the number for the # of Reservation Stations
     
      //NUMBER OF RESERVATION STATIONS
      int Num_ADD_RS = 4;
      int Num_MULT_RS = 2;
      int Num_DIV_RS = 3;
 
# b. Define latency of execution stage
    
    // RESERVATION STATION LATENCY
    const int ADD_Lat = 4;
    const int MULT_Lat = 12;
    const int DIV_Lat = 38;
    // Datapath Latency
    const int ISSUE_Lat = 1;
    const int WRITEBACK_Lat = 1;
    
# c. Initialize Reservation Station Objects
 
     //// Input reservation station architecture
     ReservationStation
             ADD1(AddOp, OperandInit),
             ADD2(AddOp, OperandInit),
             ADD3(AddOp, OperandInit),
             ADD4(AddOp, OperandInit);
     ReservationStation
             MULT1(MultOp, OperandInit),
             MULT2(MultOp, OperandInit);
     ReservationStation
             DIV1(DivOp, OperandInit),
             DIV2(DivOp, OperandInit),
             DIV3(DivOp, OperandInit);
        OperandInit = 0；
        // Pack reservation stations into vector
    vector<ReservationStation> ResStation = {ADD1,ADD2, ADD3,ADD4,MULT1,MULT2,DIV1,DIV2,DIV3}

** Note: Currently only 12 registers are being used. **

#**2. SETUP REGISTER STATUS and VALUE:**
    // Initialize register status objects
    RegisterStatus
            F0(RegStatusEmpty),F1(RegStatusEmpty),
            F2(RegStatusEmpty),F3(RegStatusEmpty),
            F4(RegStatusEmpty), F5(RegStatusEmpty),
            F6(RegStatusEmpty),F7(RegStatusEmpty),
            F8(RegStatusEmpty),F9(RegStatusEmpty),
            F10(RegStatusEmpty),F11(RegStatusEmpty),
            F12(RegStatusEmpty);
            RegStatusEmpty = 100；
    // Pack register status objects into vector
    vector<RegisterStatus> RegisterStatus =
            {F0, F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12};

    // Initialize register value vector
    vector<int> Register = {4000,1,2,3,4,5,6,7,8,9,10,11,12};

#**3. INITIALIZE TEST INSTRUCTION SETS:**

# a.Input the MIPS like instructions to your given program `Ex. ADD F1,F2,F3 // rd <- rs + rt`
  
    // Input program instructions//Test instruction 
      Instruction
              //(rd,rs,rt,opcode)
              I0(1,2,3,AddOp),       
              I1(4,1,5,AddOp),        // Data Dependency on I0
              I2(6,7,8,SubOp),       
              I3(9,4,10,MultOp),      // Data Dependency on I1
              I4(11,12,6,DivOp),      // Data Dependency on I2
              I5(8,1,5,MultOp),       // Data Dependency on I0
              I6(7,2,3,MultOp);

    // Opcode Values
             const int AddOp = 0;
             const int SubOp = 1;
             const int MultOp = 2;
             const int DivOp = 3;

#**4. OUTPUT:**
     
# a. Displays the register content of each clock cycle
    Register Content in clock 0:
    4000 1 2 3 4 5 6 7 8 9 10 11 12

    Register Content in clock 47:
    4000 5 2 3 10 5 -1 6 100 100 10 1 12
     
# b. Displays the timing diagram of the ISSUE EXECUTE WRITEBACK for each instruction at each clock cycle

    clock: 1
    Inst      Issue     Execute   WB
    0         1         0-0         0
    1         0         0-0         0
    2         0         0-0         0
    3         0         0-0         0
    4         0         0-0         0
    5         0         0-0         0
    6         0         0-0         0
    ___

    clock: 47
    Inst      Issue     Execute   WB
    0         1         2-5         6
    1         2         7-10        11
    2         3         4-7         8
    3         4         5-16        17
    4         5         9-46        47
    5         6         7-18        19
    6         18        19-30        31

# c. Displays the register status at each clcok cycle 
    Register Status in clcok 0:
    100 100 100 100 100 100 100 100 100 100 100 100 100

    Register Status in clock 7:
    100 100 100 100 1 100 100 100 5 4 100 6 100
