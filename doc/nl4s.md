# Nord Lead 4 Program File Structure (nl4s)

Offset | Size | Bits        | Description
-------|------|-------------|-----------------------------------------------------
0x000  | 4    |             | File magic, "CBIN"
0x004  | 1    | `vvvv vvvv` | File version
0x005  | 3    |             | Reserved (always zero) ?
0x008  | 4    |             | File type (generally follows file extension, "nl4s")
0x00c  | 1    | `---- -bbb` | b=bank_no(0-3)
0x00d  | 1    |             | Reserved (always zero) ?
0x00e  | 1    | `pppp pppp` | p=program_no(0-98)
0x00f  | 1    |             | Reserved (always zero) ?
0x010  | 1    | `---- ----` | ?
0x011  | 3    |             | Reserved (always zero) ?
0x014  | 1    | `---- ----` | ?
0x015  | 3    |             | Reserved (always zero) ?
0x018  | 4    |             | CRC Checksum
...    | ...  | ...         | ...
0x02e  | 1    | `ooom lggg` | o=oct_shift(0=-2, 1=-1, 2=0, 3=+1, 4=+2); m=mono(0-1); l=legato(0-1); g=glide(0-127)
0x02f  | 1    | `gggg dv--` | g=^; d=dlyvib2(0-1); v=dlyvib1(0-1)
...    | ...  | ...         | ...
0x036  | 1    | `---- --aa` | a=mod_env_attack(0-127)
0x037  | 1    | `aaaa addd` | a=^; d=mod_env_decay_release(0-127, 127=inf)
0x038  | 1    | `dddd ----` | d=^
0x039  | 1    | `---s ssri` | s=mod_env_destination(0-6)[^1]; r=mod_env_ar(0-1); i=mod_env_imp_sync(0-1) 
0x03a  | 1    | `ttt- --ww` | t=osc1_type(0-6)[^2]; w=osc1_wave(0-127)[^3]
0x03b  | 1    | `wwww wkyy` | w=^; k=osc2_kbt_off(0-1); y=osc2_type(0-6)[^2] 
0x03c  | 1    | `ysss ss--` | y=^; s=osc2_semitones(0-xxx)
0x03d  | 1    | `ffff f--t` | f=osc2_fine_tune(0-xxx); t=osc1_mod_type(0-5)[^4]
0x03e  | 1    | `ttaa aaaa` | t=^; a=osc1_mod_amount(0-127)
0x03f  | 1    | `ammm mmmm` | a=^; m=osc_mix(0-127)
0x040  | 1    | `aaaa aaad` | a=amp_env_attack(0-127); d=amp_env_decay(0-127)
0x041  | 1    | `dddd ddss` | d=^; s=amp_env_sustain(0-127)
0x042  | 1    | `ssss srrr` | s=^; r=amp_env_release(0-127)
0x043  | 1    | `rrrr vaaa` | r=^; v=amp_velocity(0-1); a=filter_attack(0-127)
0x044  | 1    | `aaaa dddd` | a=^; d=filter_decay(0-127)
0x045  | 1    | `ddds ssss` | d=^; s=filter_sustain(0-127)
0x046  | 1    | `ssrr rrrr` | s=^; r=filter_release(0-127)
0x047  | 1    | `rttt kkv-` | r=^; t=filter_type(0-6)[^5]; k=filter_kb_track(0-3); v=filter_velocity(0-1)
0x048  | 1    | `dddd dddf` | d=filter_drive(0-127); f=filter_freq(0-127)
0x049  | 1    | `ffff ffrr` | f=^; r=filter_resonance(0-127)
0x04a  | 1    | `rrrr raaa` | r=^; a=filter_env_amout(0-xxx) 
0x04b  | 1    | `aa-- oooo` | a=^; o=output(0-127)
0x04c  | 1    | `ooou um--` | o=^; u=unison(0-3); m=chord_memory(0-1)
...    | ...  | ...         | ...
0x055  | 1    | `---- e-bb` | e=hold_enable(0-1); b=bend_range(0-12)[^6]
0x056  | 1    | `bb-- ----` | b=^
0x057  | 1    | `--ff fff-` | f=osc2_noise_freq(0-xxx)
0x058  | 1    | `-rrr rrrr` | r=osc2_noise_res(0-127)
...    | ...  | ...         | ...
0x12e  | 1    | `d-c- ----` | d=delay_on(0-1); c=delay_master_clock(0-1)
0x12f  | 1    | `---- ---w` | w=dry_wet(0-127)
0x130  | 1    | `wwww wwff` | w=^; f=delay_feedback(0-3)
0x131  | 1    | `-rr- ----` | r=reverb_type(0-3)[^7]
0x132  | 1    | `--x- ttta` | x=fx_on(0-1); t=fx_type(0-5)[^8]; a=fx_amount(0-127)
0x133  | 1    | `aaaa aarb` | a=^; r=reverb_on(0-1); b=reverb_bright(0-127)
0x134  | 1    | `bbbb bb--` | b=^
0x135  | 1    | `---- -eee` | e=delay_tempo(0-127)
0x136  | 1    | `eeee ----` | e=^

[^1]: MOD ENV Destination mapping:  
    `000` (`0x0`) = OSC MIX  
    `001` (`0x1`) = OSC MOD  
    `010` (`0x2`) = OSC1  
    `011` (`0x3`) = OSC1+2  
    `100` (`0x4`) = OSC2  
    `101` (`0x5`) = FX  
    `110` (`0x6`) = LFO2  

[^2]: OSC Type mapping:  
    `000` (`0x0`) = triangle  
    `001` (`0x1`) = saw  
    `010` (`0x2`) = PWM (50%)  
    `011` (`0x3`) = square (33%)  
    `100` (`0x4`) = pulse (10%)  
    `101` (`0x5`) = wave (OSC1) / noise (OSC2)  
    `110` (`0x6`) = sine

[^3]: Not all values are valid. See wave table in `nl4_wavetable.ods`  

[^4]: OSC1 Mod Type mapping:  
    `000` (`0x0`) = mod off  
    `001` (`0x1`) = FM1  
    `010` (`0x2`) = FM2  
    `011` (`0x3`) = FM3  
    `100` (`0x4`) = S-Sync  
    `101` (`0x5`) = H-Sync

[^5]: Filter Type mapping:  
    `000` (`0x0`) = LP12  
    `001` (`0x1`) = LP24  
    `010` (`0x2`) = LP48  
    `011` (`0x3`) = BP  
    `100` (`0x4`) = HP  
    `101` (`0x5`) = Ladder M  
    `110` (`0x6`) = Ladder TB

[^6]: Bend Range mapping:  
    `0000` (`0x0`) = bend off  
    `0001` (`0x1`) = 1  
    `0010` (`0x2`) = 2  
    `0011` (`0x3`) = 3  
    `0100` (`0x4`) = 4  
    `0101` (`0x5`) = 5  
    `0110` (`0x6`) = 7  
    `0111` (`0x7`) = 10  
    `1000` (`0x8`) = 12  
    `1001` (`0x9`) = 24  
    `1010` (`0xa`) = 48  
    `1011` (`0xb`) = -12  
    `1100` (`0xc`) = -24

[^7]: Reverb Type mapping:  
    `00` (`0x0`) = None (not sure what this does)
    `01` (`0x1`) = Room
    `10` (`0x2`) = Stage
    `11` (`0x3`) = Hall

[^8]: FX Type mapping:  
    `000` (`0x0`) = Crush  
    `001` (`0x1`) = Compressor  
    `010` (`0x2`) = Drive  
    `011` (`0x3`) = Talk 1  
    `100` (`0x4`) = Talk 2  
    `101` (`0x5`) = Comb
