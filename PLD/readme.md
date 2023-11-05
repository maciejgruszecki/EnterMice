----------------------------------------------------------------------------------
-- Create Date:    13:04:42 01/17/2016 
-- Design Name:    EnterMice logic matrix
-- Module Name:    EnterMice - Behavioral 
-- Project Name:   EnterMice
-- Target Devices: XC9572XL-PC44
-- Tool versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision: 
-- Revision 0.01 - File Created
-- Additional Comments: 
--
----------------------------------------------------------------------------------
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx primitives in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity EnterMice is
    Port ( KB : in  STD_LOGIC_VECTOR (9 downto 0);
           J1_Fire1 : in  STD_LOGIC;
           J1_Fire2 : in  STD_LOGIC;
           J1_Fire3 : in  STD_LOGIC; 
           J1_Up : in  STD_LOGIC;
           J1_Down : in  STD_LOGIC;
           J1_Left : in  STD_LOGIC;
           J1_Right : in  STD_LOGIC;
           J2_Fire1 : in  STD_LOGIC;
           J2_Fire2 : in  STD_LOGIC;
           J2_Fire3 : in  STD_LOGIC;
           J2_Up : in  STD_LOGIC;
           J2_Down : in  STD_LOGIC;
           J2_Left : in  STD_LOGIC;
           J2_Right : in  STD_LOGIC;
           M_Left : in  STD_LOGIC;
           M_Right : in  STD_LOGIC;
           M_Data : in  STD_LOGIC_VECTOR (3 downto 0);
           M_JoyMode : in  STD_LOGIC; -- 0=std mouse, 1=joystick mode
           KB_J : out  STD_LOGIC;
           KB_K : out  STD_LOGIC;
           KB_L : out  STD_LOGIC);
end EnterMice;

architecture Behavioral of EnterMice is
begin												
-- inverted logic - 0's active
  KB_J  <=  (M_JoyMode or KB(0) or J1_Fire1) and                -- Joystick 1
            (M_JoyMode or KB(1) or J1_Up) and
            (M_JoyMode or KB(2) or J1_Down) and
            (M_JoyMode or KB(3) or J1_Left) and
            (M_JoyMode or KB(4) or J1_Right) and
            (KB(5) or J2_Fire1) and                             -- Joystick 2
            (KB(6) or J2_Up) and
            (KB(7) or J2_Down) and 
            (KB(8) or J2_Left) and
            (KB(9) or J2_Right) and
            (not M_JoyMode or KB(0) or M_Left) and              -- Mouse in joystick mode
            (not M_JoyMode or KB(1) or M_Data(0)) and
            (not M_JoyMode or KB(2) or M_Data(1)) and
            (not M_JoyMode or KB(3) or M_Data(2)) and
            (not M_JoyMode or KB(4) or M_Data(3));
				
  KB_K  <=  (not M_JoyMode or KB(0) or J1_Fire2) and            -- Joystick 1
            (M_JoyMode or KB(0) or J1_Fire2 or not M_Left) and  -- Mouse button priority
            (KB(5) or J2_Fire2) and                             -- Joystick 2
            (M_JoyMode or KB(0) or M_Left) and                  -- Mouse in standard mode
            (M_JoyMode or KB(1) or M_Data(0)) and		
            (M_JoyMode or KB(2) or M_Data(1)) and
            (M_JoyMode or KB(3) or M_Data(2)) and
            (M_JoyMode or KB(4) or M_Data(3));
				
  KB_L  <=  (KB(0) or J1_Fire3 or not M_Right) and              -- Joystick 1 (Mouse button priority)
	    (KB(5) or J2_Fire3) and                             -- Joystick 2
            (KB(0) or M_Right);					-- Mouse button right
end Behavioral;

