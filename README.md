# Control-CLI-Extreme
**Controlling an Extreme Networks switch by interacting with its CLI**

This module is a drop in replacement for the Control::CLI::AvayaData module following the transfer of the Avaya Data business unit to Extreme Networks.
From Control::CLI::AvayaData it inherits the same support for all of the ex-Avaya + ex-Nortel Enterprise (Bay Networks heritage) platforms and extends support to Extreme Networks switching products.
Control::CLI::Extreme currently supports:

- VSP XA-1x00, 4x00, 7x00, 8x00, 9000
- XOS Summit switches
- Universal Hardware (FabricEngine & SwitchEngine) 4120, 4220, 5520, 5420, 5320, 5720, 7520, 7720
- ERS models 2500, 3x00, 4x00, 5x00
- Wireless Wing APs and Controllers
- SLX Data Center switches
- ISW industrial switches
- Wireless HiveOS Cloud APs
- Ipanema SD-WAN appliances
- Series200 models 210, 220
- ERS/Passport models 1600, 8300, 8600, 8800
- DSG models 6248, 7648, 7480, 8032, 9032
- SR models 2330, 4134
- WLAN 91xx
- WLAN(WC) 81x0
- WLAN(WSS) 2350, 236x, 238x
- BPS 2000, ES 460, ES 470
- Baystack models 325, 425
- Accelar/Passport models 1000, 1100, 1200

The devices supported by this module can have an inconsistent CLI (in terms of syntax, login sequences, terminal width-length-paging, prompts) and in some cases two separate CLI syntaxes are available on the same product.
This class is written so that all the above products can be CLI scripted in a consistent way regardless of their underlying CLI variants. The CLI commands themselves might still vary across the different products though, even here, for certain common functions (like entering privExec mode or disabling terminal more paging) a generic method is provided by this class.

Control::CLI::Extreme is a sub-class of Control::CLI (which is required) and therefore the above functionality can also be performed in a consistent manner regardless of the underlying connection type which can be any of Telnet, SSH or Serial port connection. For SSH, only SSHv2 is supported with either password or publickey authentication.
Furthermore this module leverages the non-blocking capaility of Control::CLI version 2.00 and is thus capable of operating in a non-blocking fashion for all its methods so that it can be used to drive multiple Extreme devices simultaneusly without resorting to Perl threads (see examples directory).

Other refinements of this module over and above the basic functionality of Control::CLI are:

(i) On the stackable BaystackERS products the connect & login methods will automatically steer through the banner and menu interface (if seen) to reach the desired CLI interface.

(ii) There is no need to set the prompt string in any of this module's methods since it knows exactly what to expect from any of the supported Extreme products. Furthermore the prompt string is automatically internally set to match the actual prompt of the connected device (rather than using a generic regular expression such as '*[#>]$'). This greatly reduces the risk that the generic regular expression might trigger on a fake prompt embedded in the output stream from the device.

(iii) The connect method of this module automatically takes care of login for Telnet and Serial port access (where authentication is not part of the actual connection, unlike SSH) and so provides a consistent scripting approach whether the underlying connection is SSH or either Telnet or Serial port.

(iv) Automatic handling of output paged with --more-- prompts, including the ability to retrieve an exact number of pages of output.

(v) A number of attributes are made available to find out basic information about the connected Extreme device.

(vi) Ability to detect whether a CLI command generated an error on the remote host and ability to report success or failure of the issued command as well as the error message details.


# INSTALLATION

This module was built using Module::Build.

If you have Module::Build already installed, to install this module run the following commands:

	perl Build.PL
	./Build
	./Build test
	./Build install

Or, if you're on a platform (like DOS or Windows) that doesn't require the "./" notation, you can do this:

	perl Build.PL
	Build
	Build test
	Build install


If instead you are relying on ExtUtils::MakeMaker then run the following commands:

	perl Makefile.PL
	make
	make test
	make install

Once installed, to perform tests against Extreme devices, run the test script in interactive mode:

	perl t/extremecli.t

To perform full tests before installing the module, after build or make, run the test script like this:

	perl -Mblib t/extremecli.t

The test script also accepts command line arguments with these syntaxes:

 	extremecli.t telnet|ssh [username:password@]host [tcpPort] [blocking]
 	extremecli.t <COM-port-name> connectBaudrate[:useBaudrate] [username:password] [blocking]

	where:
		tcpPort = a valid TCP port (not 0 or 1)
		connectBaudrate = baudrate to use to connect with
		useBaudrate = once connected, change the baudrate to this new baudrate; e.g. 9600:38400 will connect @ 9600 then switch to 38400
		blocking = 0 (test non-blocking mode) or 1 (use regular blocking mode) 


# DISCLAIMER

Note that this module is in no way supported or endorsed by Extreme Networks Inc.


# SUPPORT AND DOCUMENTATION

After installing, you can find documentation for this module with the
perldoc command.

    perldoc Control::CLI::Extreme

You can also look for information at:

[RT, CPAN's request tracker](http://rt.cpan.org/NoAuth/Bugs.html?Dist=Control-CLI-Extreme)

[AnnoCPAN, Annotated CPAN documentation](http://annocpan.org/dist/Control-CLI-Extreme)

[CPAN Ratings](http://cpanratings.perl.org/d/Control-CLI-Extreme)

[Search CPAN](http://search.cpan.org/dist/Control-CLI-Extreme/)


# LICENSE AND COPYRIGHT

Copyright (C) 2024 Ludovico Stevens

This program is free software; you can redistribute it and/or modify it
under the terms of either: the GNU General Public License as published
by the Free Software Foundation; or the Artistic License.

See http://dev.perl.org/licenses/ for more information.
