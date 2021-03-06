
# udev rules
install:
	@for RULE in *-hdmi2usb*.rules; do \
		echo "Installing $$RULE to /etc/udev/rules.d/$$RULE"; \
		sudo cp $$RULE /etc/udev/rules.d/$$RULE; \
		sudo chmod 644 /etc/udev/rules.d/$$RULE; \
		sudo chown root:root /etc/udev/rules.d/$$RULE; \
	done
	@for HELP in hdmi2usb-*-helper.sh; do \
		echo "Installing $$HELP to /etc/udev/rules.d/$$HELP"; \
		sudo cp $$HELP /etc/udev/rules.d/$$HELP; \
		sudo chmod 755 /etc/udev/rules.d/$$HELP; \
		sudo chown root:root /etc/udev/rules.d/$$HELP; \
	done
	echo "udev rules installed; 'make reload' to reload udev rules"

reload:
	sudo udevadm control --reload-rules

install-reload:
	make install
	make reload

check:
	@for RULE in *-hdmi2usb*.rules; do \
		echo -n "Checking $$RULE.."; \
		[ -e /etc/udev/rules.d/$$RULE ] || exit 1; \
		diff -u $$RULE /etc/udev/rules.d/$$RULE || exit 1; \
		echo " Good!"; \
	done
	@for HELP in hdmi2usb-*-helper.sh; do \
		echo -n "Checking $$HELP.."; \
		[ -e /etc/udev/rules.d/$$HELP ] || exit 1; \
		diff -u $$HELP /etc/udev/rules.d/$$HELP || exit 1; \
		echo " Good!"; \
	done
	if ! id | grep -qF '(video)'; then \
		echo "Not a member of the video group"; exit 1; \
	fi

uninstall:
	sudo rm -f /etc/udev/rules.d/*hdmi2usb*

examine:
	make uninstall
	make install
	@echo -e "\n\n\n============================================="
	@udevadm test '/sys/class/tty/ttyACM0'
	@echo -e "\n\n\n============================================="
	@udevadm test '/sys/class/video4linux/video0'

.DEFAULT_GOAL := check
