# �ۑ� (�}���`�v���e�[�u����ǂ�) ���|�[�g

## �ۑ���e

OpenFlow1.3 �ŃX�C�b�`�̓����������悤�B

�X�C�b�`����̊e�X�e�b�v�ɂ��āAtrema dump_flows �̏o�� (�}���`�v���e�[�u���̓��e) ���������Ȃ��瓮���������邱�ƁB

## ���j

OpenFlow1.3 �ŃX�C�b�`���N�����āA����ɑ΂��Ĉȉ��̑�������s�����B�N������Ɗe����̌�� dump_flows �R�}���h�����s���āA�t���[�e�[�u���̒��g���m�F�����B

* ���� 1 : �z�X�g 1 ����z�X�g 2 �փp�P�b�g�𑗐M�B
* ���� 2 : �z�X�g 2 ����z�X�g 1 �փp�P�b�g�𑗐M�B
* ���� 3 : �Ăуz�X�g 1 ����z�X�g 2 �փp�P�b�g�𑗐M�B

## ���s���ʂƉ��

���s���ʂ��ȉ��Ɍf�ڂ���B�Ȃ��A���₷���悤�ɓK�X���s��R�����g��t���Ă���B

```
# �܂��N������̃t���[�e�[�u�����m�F

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=12.520s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=12.482s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=12.482s, table=0, n_packets=0, n_bytes=0, priority=1 actions=goto_table:1
cookie=0x0, duration=12.482s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=12.482s, table=1, n_packets=0, n_bytes=0, priority=1 actions=CONTROLLER:65535

# ���� 1

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host1 --dest host2
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=25.202s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=25.164s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=25.164s, table=0, n_packets=1, n_bytes=42, priority=1 actions=goto_table:1
cookie=0x0, duration=25.164s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=25.164s, table=1, n_packets=1, n_bytes=42, priority=1 actions=CONTROLLER:65535

# ���� 2

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host2 --dest host1
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=66.643s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=66.605s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=66.605s, table=0, n_packets=2, n_bytes=84, priority=1 actions=goto_table:1
cookie=0x0, duration=66.605s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=14.809s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=2,dl_src=19:1b:8d:39:15:d1,dl_dst=fd:0d:7a:9f:c2:36 actions=output:1
cookie=0x0, duration=66.605s, table=1, n_packets=2, n_bytes=84, priority=1 actions=CONTROLLER:65535

# ���� 3

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host1 --dest host2
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=79.756s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=79.718s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=79.718s, table=0, n_packets=3, n_bytes=126, priority=1 actions=goto_table:1
cookie=0x0, duration=79.718s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=27.922s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=2,dl_src=19:1b:8d:39:15:d1,dl_dst=fd:0d:7a:9f:c2:36 actions=output:1
cookie=0x0, duration=5.649s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=1,dl_src=fd:0d:7a:9f:c2:36,dl_dst=19:1b:8d:39:15:d1 actions=output:2
cookie=0x0, duration=79.718s, table=1, n_packets=3, n_bytes=126, priority=1 actions=CONTROLLER:65535

# �Ō�Ƀp�P�b�g���e�z�X�g�ɓ͂��Ă��邱�Ƃ��m�F

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema show_stats host1
Packets sent:
  192.168.0.1 -> 192.168.0.2 = 2 packets
Packets received:
  192.168.0.2 -> 192.168.0.1 = 1 packet
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema show_stats host2
Packets sent:
  192.168.0.2 -> 192.168.0.1 = 1 packet
Packets received:
  192.168.0.1 -> 192.168.0.2 = 2 packets

```
### �N������Ƒ��� 1

���s���ʂ�����ƁA�X�C�b�`�N������͓�̃t���[�e�[�u�� (�e�[�u�� 0 �A�e�[�u�� 1) ������B

�܂��A�e�[�u�� 0 �̓��e�͈ȉ��̒ʂ�ł���B

| �D��x | ���M�� MAC �A�h���X / �|�[�g | ���� MAC �A�h���X / �|�[�g | �A�N�V���� |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | - / -                                     | 01 ����n�܂�A�h���X / -    | �h���b�v     |
|    2     | - / -                                     | 33:33 ����n�܂�A�h���X / - | �h���b�v     |
|    1     | - / -                                     | - / -                                  | �e�[�u�� 1 �� |

��������A�e�[�u�� 0 �͓���� MAC �A�h���X���̃p�P�b�g���h���b�v����t�B���^�����O�e�[�u���ł��邱�Ƃ����������B�܂��A�e�[�u�� 0 �Œ�߂��t�B���^�ɊY�����Ȃ������p�P�b�g���e�[�u�� 1 �֓]������邱�Ƃ��A3 �Ԗڂ̃G���g������킩��B 

���Ƀe�[�u�� 1 �̓��e�͈ȉ��̒ʂ�ł���B

| �D��x | ���M�� MAC �A�h���X / �|�[�g | ���� MAC �A�h���X / �|�[�g | �A�N�V���� |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    3     | - / -                                     | ff:ff:ff:ff:ff:ff / -                 | �t���b�f�B���O |
|    1     | - / -                                     | - / -                                  | �R���g���[���֓]�� |

�����āA���� 1 ��̃t���[�e�[�u��������ƁA�V���ɉ�������G���g���͂Ȃ��B����ŁA�t���[�e�[�u���� n_packets �̓��e����A�p�P�b�g�̓X�C�b�`�ɂ���Ĉȉ��̎菇�ŏ������ꂽ���Ƃ����������B

1. �e�[�u�� 0 �ł͂�����̃t�B���^�ɂ��Y�������A�e�[�u�� 1 �֓]�������B
2. �e�[�u�� 1 �� 2 �Ԗڂ̃G���g���ɂ��A�R���g���[���� Packet In �����B 

���̌�A�R���g���[���� FDB �ɂ́A�z�X�g 2 �Ɛڑ�����|�[�g�̃G���g�����Ȃ����߁A�R���g���[���̓p�P�b�g���t���b�f�B���O����B�����ɁA�z�X�g 1 �ƃX�C�b�`�̃|�[�g 1 �̑Ή��� FDB �ɓo�^�����B

### ���� 2

�����āA���� 2 ��̃t���[�e�[�u��������ƁA�V���Ɉȉ��̃G���g�����e�[�u�� 1 �֒ǉ�����Ă��邱�Ƃ�������B

| �D��x | ���M�� MAC �A�h���X / �|�[�g | ���� MAC �A�h���X / �|�[�g | �A�N�V���� |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | 19:1b:8d:39:15:d1 (�z�X�g 2) / 2 | fd:0d:7a:9f:c2:36 (�z�X�g 1) / - | �|�[�g 1 �֑��M |

�z�X�g 2 ����X�C�b�`�ɗ��������p�P�b�g�́A���� 1 �̎��Ɠ��l�ɃR���g���[���� Packet In �����B�R���g���[���� FDB �ɂ̓z�X�g 1 �ƃX�C�b�`�̃|�[�g 1 �̑Ή���\���G���g��������̂ŁA�R���g���[���̓|�[�g 1 �� Packet Out ���A�����ɏ�L�̃t���[�G���g�����e�[�u�� 1 �֒ǉ��������̂ƍl������B�܂����̎��A�z�X�g 2 �ƃX�C�b�`�̃|�[�g 2 �̑Ή��� FDB �ɓo�^�����B

### ���� 3

�����āA���� 3 ��̃t���[�e�[�u��������ƁA�V���Ɉȉ��̃G���g�����e�[�u�� 1 �֒ǉ�����Ă��邱�Ƃ��킩��B

| �D��x | ���M�� MAC �A�h���X / �|�[�g | ���� MAC �A�h���X / �|�[�g | �A�N�V���� |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | fd:0d:7a:9f:c2:36 (�z�X�g 1) / 1 | 19:1b:8d:39:15:d1 (�z�X�g 2) / - | �|�[�g 2 �֑��M |

�z�X�g 1 ����X�C�b�`�ɗ��������p�P�b�g�́A���� 2 �̎��Ɠ��l�ɃR���g���[���� Packet In �����B�R���g���[���� FDB �ɂ̓z�X�g 2 �ƃX�C�b�`�̃|�[�g 2 �̑Ή���\���G���g��������̂ŁA�R���g���[���̓|�[�g 2 �� Packet Out ���A�����ɏ�L�̃t���[�G���g�����e�[�u�� 1 �֒ǉ��������̂ƍl������B