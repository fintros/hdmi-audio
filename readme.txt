---------------------------------------------------------------------------
-- (c) 2016 Alexey Spirkov
-- I am happy for anyone to use this for non-commercial use.
-- If my verilog/vhdl/c files are used commercially or otherwise sold,
-- please contact me for explicit permission at me _at_ alsp.net.
-- This applies for source and binary form and derived works.
---------------------------------------------------------------------------
-- Recommended params:
-- N=0x1800 CTS=0x6FD1 (28.625MHz pixel clock -> 48KHz audio clock)
-- N=0x1000 CTS=0x6FD1 (28.625MHz pixel clock -> 32KHz audio clock)
-- N=0x1000 CTS=0x6978 (27MHz pixel clock -> 32KHz audio clock)
-- N=0x1800 CTS=0x6978 (27MHz pixel clock -> 48KHz audio clock)
---------------------------------------------------------------------------

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity av_hdmi is
generic
(
	FREQ: integer := 27000000;	-- pixel clock frequency
	FS: integer := 48000;		-- audio sample rate - should be 32000, 41000 or 48000
	CTS: integer := 27000;		-- CTS = Freq(pixclk) * N / (128 * Fs)
	N: integer := 6144		-- N = 128 * Fs /1000,  128 * Fs /1500 <= N <= 128 * Fs /300
					-- Check HDMI spec 7.2 for details
);
port (
	-- clocks
	I_CLK_PIXEL_x5	: in std_logic;
	I_CLK_PIXEL		: in std_logic;
	
	-- components
	I_R			: in std_logic_vector(7 downto 0);
	I_G			: in std_logic_vector(7 downto 0);
	I_B			: in std_logic_vector(7 downto 0);	
	I_BLANK			: in std_logic;
	I_HSYNC			: in std_logic;
	I_VSYNC			: in std_logic;
	
	-- PCM audio 
	I_AUDIO_PCM_L 		: in std_logic_vector(15 downto 0);
	I_AUDIO_PCM_R		: in std_logic_vector(15 downto 0);
	
	-- TMDS output
	O_TMDS_D0		: out std_logic;
	O_TMDS_D1		: out std_logic;
	O_TMDS_D2		: out std_logic;
	O_TMDS_CLK		: out std_logic);
end entity;
