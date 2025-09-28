import java.util.HashMap;
import java.util.Map;

// === Command Interface ===
interface Command {
    void execute();
}

// === Central Hub ===
class CentralHub {
    private final Map<String, Command> commands = new HashMap<>();

    public void registerCommand(String action, Command command) {
        commands.put(action, command);
    }

    public void sendCommand(String action) {
        Command command = commands.get(action);
        if (command != null) {
            command.execute();
        } else {
            System.out.println("Unknown command: " + action);
        }
    }
}

// === Devices ===
class Light {
    private final String id;
    private int brightness;
    private boolean on;

    public Light(String id) {
        this.id = id;
    }

    public void setBrightness(int brightness) {
        this.brightness = brightness;
        System.out.println(id + ": Brightness set to " + brightness + "%");
    }

    public void on() {
        if (!on) {
            on = true;
            System.out.println(id + ": Light is ON (brightness=" + brightness + ")");
        } else {
            System.out.println(id + ": Light is already ON.");
        }
    }

    public void off() {
        if (on) {
            on = false;
            System.out.println(id + ": Light is OFF");
        } else {
            System.out.println(id + ": Light is already OFF.");
        }
    }

    public boolean isOn() {
        return on;
    }
}

class Thermostat {
    private final String id;
    private double temperature;

    public Thermostat(String id) {
        this.id = id;
    }

    public void setTemperature(double temperature) {
        this.temperature = temperature;
        System.out.println(id + ": Thermostat set to " + temperature + "°C");
    }

    public void increase(double delta) {
        this.temperature += delta;
        System.out.println(id + ": Thermostat increased to " + temperature + "°C");
    }

    public void decrease(double delta) {
        this.temperature -= delta;
        System.out.println(id + ": Thermostat decreased to " + temperature + "°C");
    }
}

class MusicPlayer {
    private final String id;
    private int volume;
    private String playlist;
    private boolean playing;

    public MusicPlayer(String id) {
        this.id = id;
    }

    public void setPlaylist(String playlist) {
        this.playlist = playlist;
        System.out.println(id + ": Playlist set to '" + playlist + "'");
    }

    public void play() {
        if (!playing) {
            playing = true;
            System.out.println(id + ": Playing playlist '" + playlist + "' at volume " + volume);
        } else {
            System.out.println(id + ": Already playing.");
        }
    }

    public void stop() {
        if (playing) {
            playing = false;
            System.out.println(id + ": Stopped playback");
        } else {
            System.out.println(id + ": Already stopped.");
        }
    }

    public void increaseVolume(int delta) {
        this.volume += delta;
        System.out.println(id + ": Volume increased to " + volume);
    }

    public void decreaseVolume(int delta) {
        this.volume -= delta;
        System.out.println(id + ": Volume decreased to " + volume);
    }

    public boolean isPlaying() {
        return playing;
    }
}

class SmartBlinds {
    private final String id;
    private int angle;

    public SmartBlinds(String id) {
        this.id = id;
    }

    public void open() {
        this.angle = 100;
        System.out.println(id + ": Blinds opened (angle=" + angle + ")");
    }

    public void close() {
        this.angle = 0;
        System.out.println(id + ": Blinds closed (angle=" + angle + ")");
    }

    public void setAngle(int angle) {
        this.angle = angle;
        System.out.println(id + ": Blinds angle set to " + angle);
    }
}

class SmartFan {
    private final String id;
    private int speed; // 0 = off, 1–5 = speed levels

    public SmartFan(String id) {
        this.id = id;
    }

    public void on() {
        if (speed == 0) speed = 1;
        System.out.println(id + ": Fan turned ON at speed " + speed);
    }

    public void off() {
        speed = 0;
        System.out.println(id + ": Fan turned OFF");
    }

    public void setSpeed(int speed) {
        this.speed = speed;
        System.out.println(id + ": Fan speed set to " + speed);
    }
}

// === Command Implementations ===
// Light Commands
class LightOnCommand implements Command {
    private final Light light;
    private final int brightness;

    public LightOnCommand(Light light, int brightness) {
        this.light = light;
        this.brightness = brightness;
    }

    public void execute() {
        light.setBrightness(brightness);
        light.on();
    }
}

class LightOffCommand implements Command {
    private final Light light;
    public LightOffCommand(Light light) { this.light = light; }
    public void execute() { light.off(); }
}

// Thermostat Commands
class SetTemperatureCommand implements Command {
    private final Thermostat thermostat;
    private final double temperature;

    public SetTemperatureCommand(Thermostat thermostat, double temperature) {
        this.thermostat = thermostat;
        this.temperature = temperature;
    }

    public void execute() { thermostat.setTemperature(temperature); }
}

class IncreaseTemperatureCommand implements Command {
    private final Thermostat thermostat;
    private final double delta;

    public IncreaseTemperatureCommand(Thermostat thermostat, double delta) {
        this.thermostat = thermostat;
        this.delta = delta;
    }

    public void execute() { thermostat.increase(delta); }
}

class DecreaseTemperatureCommand implements Command {
    private final Thermostat thermostat;
    private final double delta;

    public DecreaseTemperatureCommand(Thermostat thermostat, double delta) {
        this.thermostat = thermostat;
        this.delta = delta;
    }

    public void execute() { thermostat.decrease(delta); }
}

// Music Player Commands
class PlayMusicCommand implements Command {
    private final MusicPlayer player;
    private final String playlist;

    public PlayMusicCommand(MusicPlayer player, String playlist) {
        this.player = player;
        this.playlist = playlist;
    }

    public void execute() {
        player.setPlaylist(playlist);
        player.play();
    }
}

class StopMusicCommand implements Command {
    private final MusicPlayer player;
    public StopMusicCommand(MusicPlayer player) { this.player = player; }
    public void execute() { player.stop(); }
}

class IncreaseVolumeCommand implements Command {
    private final MusicPlayer player;
    private final int delta;

    public IncreaseVolumeCommand(MusicPlayer player, int delta) {
        this.player = player;
        this.delta = delta;
    }

    public void execute() { player.increaseVolume(delta); }
}

class DecreaseVolumeCommand implements Command {
    private final MusicPlayer player;
    private final int delta;

    public DecreaseVolumeCommand(MusicPlayer player, int delta) {
        this.player = player;
        this.delta = delta;
    }

    public void execute() { player.decreaseVolume(delta); }
}

// SmartBlinds Commands
class OpenBlindsCommand implements Command {
    private final SmartBlinds blinds;
    public OpenBlindsCommand(SmartBlinds blinds) { this.blinds = blinds; }
    public void execute() { blinds.open(); }
}

class CloseBlindsCommand implements Command {
    private final SmartBlinds blinds;
    public CloseBlindsCommand(SmartBlinds blinds) { this.blinds = blinds; }
    public void execute() { blinds.close(); }
}

class SetBlindsAngleCommand implements Command {
    private final SmartBlinds blinds;
    private final int angle;

    public SetBlindsAngleCommand(SmartBlinds blinds, int angle) {
        this.blinds = blinds;
        this.angle = angle;
    }

    public void execute() { blinds.setAngle(angle); }
}

// SmartFan Commands
class FanOnCommand implements Command {
    private final SmartFan fan;
    public FanOnCommand(SmartFan fan) { this.fan = fan; }
    public void execute() { fan.on(); }
}

class FanOffCommand implements Command {
    private final SmartFan fan;
    public FanOffCommand(SmartFan fan) { this.fan = fan; }
    public void execute() { fan.off(); }
}

class SetFanSpeedCommand implements Command {
    private final SmartFan fan;
    private final int speed;

    public SetFanSpeedCommand(SmartFan fan, int speed) {
        this.fan = fan;
        this.speed = speed;
    }

    public void execute() { fan.setSpeed(speed); }
}

// === Demo Main ===
public class SmartHome {
    public static void main(String[] args) {
        CentralHub hub = new CentralHub();

        // Devices
        Light livingRoomLight = new Light("LivingRoomLight");
        MusicPlayer kitchenPlayer = new MusicPlayer("KitchenPlayer");
        Thermostat mainThermo = new Thermostat("MainThermo");
        SmartBlinds windowBlinds = new SmartBlinds("MainWindowBlinds");
        SmartFan ceilingFan = new SmartFan("CeilingFan");

        // Register Light Commands
        hub.registerCommand("livingroom.light.on", new LightOnCommand(livingRoomLight, 80));
        hub.registerCommand("livingroom.light.off", new LightOffCommand(livingRoomLight));

        // Register Music Player Commands
        hub.registerCommand("music.play", new PlayMusicCommand(kitchenPlayer, "Rock Classics"));
        hub.registerCommand("music.stop", new StopMusicCommand(kitchenPlayer));
        hub.registerCommand("music.volume.up", new IncreaseVolumeCommand(kitchenPlayer, 10));
        hub.registerCommand("music.volume.down", new DecreaseVolumeCommand(kitchenPlayer, 10));

        // Register Thermostat Commands
        hub.registerCommand("thermostat.set.24", new SetTemperatureCommand(mainThermo, 24.0));
        hub.registerCommand("thermostat.increase.2", new IncreaseTemperatureCommand(mainThermo, 2.0));
        hub.registerCommand("thermostat.decrease.2", new DecreaseTemperatureCommand(mainThermo, 2.0));

        // Register Blinds Commands
        hub.registerCommand("blinds.open", new OpenBlindsCommand(windowBlinds));
        hub.registerCommand("blinds.close", new CloseBlindsCommand(windowBlinds));
        hub.registerCommand("blinds.angle.30", new SetBlindsAngleCommand(windowBlinds, 30));

        // Register Fan Commands
        hub.registerCommand("fan.on", new FanOnCommand(ceilingFan));
        hub.registerCommand("fan.off", new FanOffCommand(ceilingFan));
        hub.registerCommand("fan.speed.3", new SetFanSpeedCommand(ceilingFan, 3));

        // === Simulate commands ===
        hub.sendCommand("livingroom.light.on");
        hub.sendCommand("music.play");
        hub.sendCommand("music.volume.up");
        hub.sendCommand("thermostat.set.24");
        hub.sendCommand("blinds.angle.30");
        hub.sendCommand("fan.on");
        hub.sendCommand("fan.speed.3");
        hub.sendCommand("fan.off");
        hub.sendCommand("livingroom.light.off");
        hub.sendCommand("music.stop");
    }
}
