<template>
    <v-app dark>
        <Toolbar/>

        <v-content>
            <v-container>
                <v-layout>
                    <v-flex offset-sm2
                            :class="{'xs12': $vuetify.breakpoint.smAndDown, 'sm8': $vuetify.breakpoint.mdAndUp}">
                        <v-card>
                            <v-img height="400px"
                                   :src="songs[currentSong.index].cover">
                            </v-img>
                            <v-card-title>
                                <div>
                                    <h3 class="grey--text">{{ songs[currentSong.index].artist }}</h3>
                                    <h2>{{ songs[currentSong.index].title }}</h2>
                                </div>
                            </v-card-title>
                            <v-card-actions class="pb-4">

                                <v-layout row wrap>
                                    <v-flex md5 sm12 class="text-xs-center">
                                        <div class="pr-3 pl-3">
                                            <v-layout class="align-center">
                                                <v-flex>
                                                    <v-btn outline small fab color="pink" @click="volumeMute">
                                                        <v-icon v-if="!mute">volume_up</v-icon>
                                                        <v-icon v-else>volume_off</v-icon>
                                                    </v-btn>
                                                </v-flex>
                                                <v-flex>
                                                    <v-slider @input="volumeUpdate(volume)"
                                                              :class="[{'ml-0': $vuetify.breakpoint.smAndDown, 'ml-3': $vuetify.breakpoint.mdAndUp}]"
                                                              color="pink"
                                                              v-model="volume"
                                                              max="1" step="0.1"
                                                    ></v-slider>
                                                </v-flex>
                                            </v-layout>
                                        </div>
                                    </v-flex>

                                    <v-flex md7 sm12 class="text-xs-center">
                                        <v-btn outline small fab color="white" @click="stop">
                                            <v-icon>stop</v-icon>
                                        </v-btn>
                                        <v-btn outline fab color="white" @click="prev">
                                            <v-icon>skip_previous</v-icon>
                                        </v-btn>
                                        <v-btn outline large fab color="pink" @click="playOrPause">
                                            <v-icon v-if="isPlaying">pause</v-icon>
                                            <v-icon v-else>play_arrow</v-icon>
                                        </v-btn>
                                        <v-btn outline fab color="white" @click="next">
                                            <v-icon>skip_next</v-icon>
                                        </v-btn>
                                        <v-btn outline small fab :color="repeatColor" @click="repeat = !repeat">
                                            <v-icon v-if="!repeat">repeat</v-icon>
                                            <v-icon v-else>repeat_one</v-icon>
                                        </v-btn>
                                    </v-flex>
                                </v-layout>

                            </v-card-actions>

                            <div class="pr-3 pl-3">
                                <v-layout class="align-center">
                                    <v-flex shrink style="width: 60px">
                                        <div class="text--white text-xs-center">{{ currentSong.currentSongTime.minute +
                                            ' : ' + currentSong.currentSongTime.second }}
                                        </div>
                                    </v-flex>
                                    <v-flex>
                                        <v-slider @change="seekUpdate" v-model="currentSong.currentSongTime.total"
                                                  :max="currentSong.duration.total" :step="1"></v-slider>
                                    </v-flex>
                                    <v-flex shrink style="width: 60px">
                                        <div class="text--white text-xs-center">{{ currentSong.duration.minute + ' : ' +
                                            currentSong.duration.second }}
                                        </div>
                                    </v-flex>
                                </v-layout>
                            </div>

                            <v-list two-line>
                                <template v-for="(song, index) in songs">
                                    <v-divider :key="index"></v-divider>
                                    <v-list-tile avatar @click="getCurrentSong(index)"
                                                 :class="{'pink': currentSong.index === index}">
                                        <v-list-tile-avatar tile>
                                            <img :src="song.cover">
                                        </v-list-tile-avatar>
                                        <v-list-tile-content>
                                            <v-list-tile-title v-html="song.title"></v-list-tile-title>
                                            <v-list-tile-sub-title v-html="song.artist"></v-list-tile-sub-title>
                                        </v-list-tile-content>
                                    </v-list-tile>
                                </template>
                            </v-list>
                        </v-card>
                        <div class="text-xs-center mt-5 grey--text">
                            <h2 class="mb-3">Made by Morteza Sabihi</h2>
                            <a class="github-button" href="https://github.com/mortezasabihi/vue_music_player"
                               data-icon="octicon-star" data-size="large"
                               aria-label="Star mortezasabihi/vue_music_player on GitHub">Star</a>
                        </div>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-content>

    </v-app>
</template>

<script>
    import Toolbar from "./components/Toolbar";

    export default {
        name: 'App',
        components: {Toolbar},
        comments: {
            Toolbar
        },
        data() {
            return {
                songs: [
                    {
                        artist: "Ashvan",
                        title: "Maghrour",
                        src: "http://media.mtvpersian.net/2018/Dec/23/Ashvan%20-%20Maghrour.mp3",
                        cover: "https://www.mtvpersian.net/main/uploads/covers/thumbs/17001_17001_400.jpg"
                    },
                    {
                        artist: "Mehdi Jahani",
                        title: "Doost Daram In Havaro",
                        src: "http://media.mtvpersian.net/2019/Jan/13/Mehdi%20Jahani%20-%20Doost%20Daram%20In%20Havaro.mp3",
                        cover: "https://www.mtvpersian.net/main/uploads/covers/thumbs/17053_17053_400.jpg"
                    },
                    {
                        artist: "Emad Talebzadeh",
                        title: "Maghrour",
                        src: "http://media.mtvpersian.net/2019/Mar/19/Emad%20Talebzadeh%20-%20Maghrour.mp3",
                        cover: "https://www.mtvpersian.net/covers/thumbs2/17391_17391_400.jpg"
                    },
                    {
                        artist: "Hamid Hiraad",
                        title: "Codeine",
                        src: "http://media.mtvpersian.net/2019/Mar/19/Hamid%20Hiraad%20-%20Codeine.mp3",
                        cover: "https://www.mtvpersian.net/covers/thumbs2/17393_17393_400.jpg"
                    },
                    {
                        artist: "Mohsen Chavoshi",
                        title: "Halalam Kon",
                        src: "http://media.mtvpersian.net/2019/Mar/19/Mohsen%20Chavoshi%20-%20Halalam%20Kon.mp3",
                        cover: "https://www.mtvpersian.net/covers/thumbs2/17392_17392_400.jpg"
                    },
                    {
                        artist: "EMO Band",
                        title: "Delam Mire Barat (Remix)",
                        src: "http://media.mtvpersian.net/2019/Mar/20/EMO%20Band%20-%20Delam%20Mire%20Barat%20(Remix).mp3",
                        cover: "https://www.mtvpersian.net/covers/thumbs2/17399_17399_400.jpg"
                    },
                    {
                        artist: "Reza Sadeghi",
                        title: "Boghzo Baroot",
                        src: "http://media.mtvpersian.net/2018/Dec/29/Reza%20Sadeghi%20-%20Boghzo%20Baroot.mp3",
                        cover: "https://www.mtvpersian.net/main/uploads/covers/thumbs/17023_17023_400.jpg"
                    },
                    {
                        artist: "Reza Sadeghi",
                        title: "Nafas",
                        src: "http://media.mtvpersian.net/mp3/2018/Mar/01/Reza%20Sadeghi%20-%20Nafas.mp3",
                        cover: "https://www.mtvpersian.net/main/uploads/covers/thumbs/14338_14338_400.jpg"
                    },
                ],
                volume: 1,
                currentSong: {
                    index: 0,
                    src: '',
                    duration: {
                        second: 0,
                        minute: 0,
                        total: 0
                    },
                    currentSongTime: {
                        second: 0,
                        minute: 0,
                        total: 0
                    }
                },
                isPlaying: false,
                audio: '',
                mute: false,
                repeat: false
            }
        },
        watch: {
            repeat(val) {
                let self = this;
                this.audio.addEventListener("ended", function () {
                    if (val) {
                        self.prev();
                        console.log('repeated');
                    } else {
                        self.next();
                    }
                }, false);
            }
        },
        created() {
            let self = this;
            this.currentSong.src = this.songs[this.currentSong.index].src;
            this.audio = new Audio(this.currentSong.src);

            this.audio.addEventListener("ended", function () {
                self.next();
            });

            this.audio.addEventListener("canplaythrough", function () {
                self.setDuration();
            });

            this.audio.addEventListener("timeupdate", function () {
                self.timeUpdate();
            });
        },
        methods: {
            getCurrentSong(id) {
                this.currentSong.index = id;
                this.currentSong.src = this.songs[this.currentSong.index].src;
                this.changeSong();
            },
            playOrPause() {
                this.isPlaying = !this.isPlaying;
                if (this.isPlaying) {
                    this.audio.play();
                } else {
                    this.audio.pause();
                }
            },
            next() {
                if (this.currentSong.index < this.songs.length - 1) {
                    this.currentSong.index++;
                } else {
                    this.currentSong.index = 0;
                }
                this.changeSong();
            },
            prev() {
                if (this.currentSong.index > 0) {
                    this.currentSong.index--;
                } else {
                    this.currentSong.index = this.songs.length - 1;
                }
                this.changeSong();
            },
            stop() {
                this.audio.currentTime = 0;
                this.audio.pause();
                this.isPlaying = false;
            },
            changeSong() {
                this.currentSong.src = this.songs[this.currentSong.index].src;
                this.audio.setAttribute('src', this.currentSong.src);
                this.audio.play();
                this.isPlaying = true;
            },
            volumeUpdate(vol) {
                this.audio.volume = vol;
            },
            volumeMute() {
                this.mute = !this.mute;
                if (this.mute) {
                    this.audio.muted = true;
                } else {
                    this.audio.muted = false;
                }
            },
            setDuration() {
                this.currentSong.duration.second = parseInt(this.audio.duration % 60);
                this.currentSong.duration.minute = parseInt((this.audio.duration / 60) % 60);
                this.currentSong.duration.total = Math.floor(this.audio.duration);
            },
            timeUpdate() {
                this.currentSong.currentSongTime.second = parseInt(this.audio.currentTime % 60);
                this.currentSong.currentSongTime.minute = parseInt((this.audio.currentTime / 60) % 60);
                this.currentSong.currentSongTime.total = this.audio.currentTime;
            },
            seekUpdate(value) {
                this.audio.currentTime = value;
            },
        },
        computed: {
            repeatColor() {
                if (this.repeat) {
                    return "pink";
                } else {
                    return "white";
                }
            }
        }
    }
</script>
