# Hack-Computer-Implementation

// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.

/**
 * The Hack CPU (Central Processing unit), consisting of an ALU,
 * two registers named A and D, and a program counter named PC.
 * The CPU is designed to fetch and execute instructions written in 
 * the Hack machine language. In particular, functions as follows:
 * Executes the inputted instruction according to the Hack machine 
 * language specification. The D and A in the language specification
 * refer to CPU-resident registers, while M refers to the external
 * memory location addressed by A, i.e. to Memory[A]. The inM input 
 * holds the value of this location. If the current instruction needs 
 * to write a value to M, the value is placed in outM, the address 
 * of the target location is placed in the addressM output, and the 
 * writeM control bit is asserted. (When writeM==0, any value may 
 * appear in outM). The outM and writeM outputs are combinational: 
 * they are affected instantaneously by the execution of the current 
 * instruction. The addressM and pc outputs are clocked: although they 
 * are affected by the execution of the current instruction, they commit 
 * to their new values only in the next time step. If reset==1 then the 
 * CPU jumps to address 0 (i.e. pc is set to 0 in next time step) rather 
 * than to the address resulting from executing the current instruction. 
 */

CHIP CPU {

    IN  inM[16],         // M value input  (M = contents of RAM[A])
        instruction[16], // Instruction for execution
        reset;           // Signals whether to re-start the current
                         // program (reset==1) or continue executing
                         // the current program (reset==0).

    OUT outM[16],        // M value output
        writeM,          // Write to M? 
        addressM[15],    // Address in data memory (of M)
        pc[15];          // address of next instruction

    PARTS:
	//1. Mux16 for ARegister
	Mux16(a= , b= ,sel= ,out =  );
	
	//2. ARegister: generate the load bit first

	ARegister(in= , out= , out[0..14]=addressM, load= );

	//3. Mux16 for ALU
	Mux16(a= , b= , sel= , out= );

	//4. DRegister:generate the load bit first

    	DRegister(in= ,out= ,load= );

	//5. ALU
	ALU(x= ,y= ,
	out= ,out= ,
	zx= ,nx= ,
	zy= ,ny= ,
	f= ,no= ,
	ng= ,zr= );
	
	//6. PC: generate load bit first
	PC(in= , load= , inc= , reset= ,out[0..14]=pc);	

	//7. writeM
	

}
