# SDSU CS370 Project 3 

Participants 

    Teague Sangster
    Noah Bakayou
    Sean Hedgecock
    Aybars Seyrek 

## Project Parts Breakdown:
![Screenshot 2024-12-08 235257](https://github.com/user-attachments/assets/0b56dbe5-a2ad-41e4-90c6-7be040bfc444)
![Screenshot 2024-12-08 235330](https://github.com/user-attachments/assets/87968916-6f62-492b-92bc-9f09212c61f8)
![Screenshot 2024-12-08 235408](https://github.com/user-attachments/assets/ebadbb90-afef-40d5-9daa-f648e3082d39)

### Part 1: Finite State Bike Lock 

    Here we were tasked with making a FSM (Finite State Machine) that takes the code 01100000, 
    and gives the user 16 tries to implement it before they are locked out. 
    In this case we used D-Flip Flops to hold which stage of the input the users were on, 
    and a Lock to keep track of how many inputs out of our 16 our user has used up. 
    In making our FSM we had to break out project down into the following sub components:

    1. State Drawing and State Table 
        Lab 3 part 1 started by developing a comprehensive state diagram based on the the Red ID secret key 01100000. 
        We then identified and labeled eight distinct states (S0-S7) and analyzed the transitions between these states. 
        After constructing the state diagram, we derived the corresponding state table to accurately represent the next-state output logic. 
        This allowed for subsequent implementation steps into the K-maps.

    2. Turning our state table into a Circut:
        Here we use our foundations from the semester and build truth tables for our next state. 
        Our next states are the functions our Truth Tables hold and activate depending how along in the code they are. 
        This allows for the D-Flip-Flops to hold both the current code, and where the code should go given they input the correct next choice. 
        Our K-Maps were then turned into their literal functions (And Or Xor) and wired with the current state,
         next state, and finally a lock to stop input after 16 attempts.

    3. Lock 
        Our 16-bit lock circuit uses 5 D flip-flops to control access by counting input pulses. 
        The input pulses represent each time a bit (0 or 1) is entered into the circuit, triggering state changes in the flip-flops. 
        To ensure a pulse is generated regardless of the input (0 or 1), we use a NOT gate on the 0 button and OR that with the 1 button signal. 
        This ensures that a 1 is consistently sent through as a pulse, whenever a button is pressed. 
        These pulses are received by the flip-flops, which work together to manage the sequence of operations. 
        The circuit processes 16 bits in two distinct stages: the first 8 bits and the next 8 bits.

        The first three flip-flops (FF1, FF2, and FF3) form a mod-8 counter. 
        With each pulse, these flip-flops count up to 8 in binary (000 to 111). 
        On the 8th pulse, FF3 outputs a 1, signaling the completion of the first stage. 
        This output triggers the fourth flip-flop (FF4), which sets its state to 1 and enables the second stage. 
        The fifth flip-flop (FF5) then begins processing the next 8 pulses independently. 
        After completing these 8 pulses, FF5 generates a final output signal. 
        Once all 16 bits are processed, this output signal is used to prevent additional 
        inputs from passing through the final AND gate, ensuring the circuit locks after the 16-bit sequence is completed.
    

    All of these parts 

    Part 2: User Programable Bike Lock 

### Part 2: User Programable State Bike Lock 
![Screenshot 2024-12-08 235429](https://github.com/user-attachments/assets/8a0306e5-c74f-44ed-8a7a-ddc6341dc9c4)
![Screenshot 2024-12-08 235508](https://github.com/user-attachments/assets/7af298db-c4f4-42a9-ad59-c925747c6cf3)
![Screenshot 2024-12-08 235408](https://github.com/user-attachments/assets/29449df4-7fa0-401f-8c26-f7577043599b)

    Part 2 uses the same basis for the logic and builds on it, using an 8 Bit Shift Register (Our User Attempted Code Storer) and 
    multiple D-Flip-Flops to hold the signal being passed to the system. 
    In this case the user needs to be able to set what the code is, meaning our Finite State Logic needs to change. 
    To do this without making the lock always able to be overwritten we have an enable button to set our D-Flip-Flop's (Which we will later refer to as our "Look Up Table") (FF),
    which then hold that state for the rest of the time unil it is reset. 
    
    Here we can break it down into a few major components:

    1. Shift Registers (User Attempt Storage):
        An 8-bit shift register is constructed by connecting eight D flip-flops in series, 
        where each flip-flop holds a single bit of data. On each clock pulse (Or in this case when they add more Data), 
        the data bit at the input of the first flip-flop is transferred to its output,
        and each subsequent flip-flop passes the bit from its input to its output, 
        effectively shifting the data one position to the right.
        This all sounds pretty complicated but this exists just to keep track of what the user is trying to input as their password.    
        This setup allows for serial input of data,
        where each bit can then be accessed seperatly and compared agasint our saved password. 
        (TLDR: Users code's are stored here for comparison with the rest of the circut)

    2. Series of D-Flip-Flops to hold our code. 
        At the very begining before it makes sense for any other buttons to be pressed,
        the user can set the "Password" using the 8 D-Flip Flops. 
        These can only be overwritten when the Enable pin is 1, 
        and they will hold their internal Signal (What they are programed for) until they are told (Enabled) to reset. 
        For future refernce we're gonna refer to this as our "Look up Table" as this is where our user code is stored and what we will compare against later on. 

    3. Lock 
        Here our lock is back! It keeps track of the button pulses, 
        and then fianlly sends a 1 to lock all of the FF's from changing after a user presses them.
        This lock is still couting up to 16 so if they can't implement it in 16 tries they're locked out. 
    
    4. Series of XNOR Gates (Our Code Varification and comparison)
        An XNOR gate, or "exclusive NOR," outputs a 1 when both inputs are the same—either both 0 or both 1.
        In this case we use it to make sure both our "Look up Table" and our user inputs match. 
        As long as they both say 0 or 1, we consider it a success! (The user successfully matched the stored code)
        To be more specific our user inputs are being held in our 8 bit shift register, where each one of
        it's 8 bit pins are being compared against the "Look Up Table" (The 8 D-Flip-Flops) using 8 XNOR's.

    5. Fianlly the AND gate
        Our last step is making sure all of our "Checks" (XNOR's) returned 1's, and what better to do that than an AND!
        This is the last part of our project and this part ensures that all of our codes match before returning a 1. '
    
    All together part 2 works by storing both the Desired code as well as the users attempted at the code. 
    Which is then compares using XNOR's and an AND on all the XNOR's Checking.

## Who Did What Work:
    Teague:
        Part 1:
            Built K-Maps and the inital Flip flop design for part 1.
            Build the wiring and the layout for the Button D-Flop for part 1 
        Part 2:
            Built the outline for part 2. 
            Completed General Wiring 
            Added The D-Flop Circuts and Sift Registers from user inputs
            Implemented the, buttons and Registers for part 2.
            

    Sean:
        I contributed to Lab 3 by developing a state diagram based on the the Red ID secret key 01100000. 
        I then identified and labeled eight distinct states (S0-S7) and 
        analyzed the transitions between these states. After constructing the state diagram,
        I derived the corresponding state table to accurately represent the next-state output logic. 
        This allowed for subsequent implementation steps into the K-maps.

    Noah: 
        Part 1
            16 bit lock
            Exported and labeled parts 
            Labeled The K-Map Implementations
        Part 2
            D flip flops in parallel
            Debugged shift register logic
            Clock working
            Added XNOR 
            Fixed Wiring 
            Exported Parts and Added Labels 

    Aybars:
        Added Comments for group readability 
        Provided General Information
        Wrote Summaries for different components
    
        
        
