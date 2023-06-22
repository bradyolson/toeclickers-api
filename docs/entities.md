## `Profile`
* profile_uuid: string
* region: string
* name: string
* tag: string
* player_card_uuid: string

## `Agent`
* agent_uuid: string
* name: string
* description: string

* one to many `AgentAbility`s by 

## `AgentAbility`
* agent_ability_uuid: string
* agent_uuid: string
* slot: string
* display_name: string
* description: string
* display_icon: string

## `Weapon`
* weapon_uuid: string
* display_name: string
* display_icon: string
* killfeed_icon: string

## `Map`
* map_uuid: string
* display_name: string
* display_icon: string
* list_icon: string
* splash_image: string
* x_multiplier: float
  * x coordinate multiplier for event locations
* y_multiplier: float
  * y coordinate multiplier for event locations
* x_scalar: float
  * x coordinate scalar offset for event locations
* y_scalar: float
  * y coordinate scalar offset for event locations

## `ProfileMmr`
* profile_uuid: string
* tier: MmrTier
* rr_in_tier: int
* total_rr: int
* games_needed_for_rating: int

* one to many `SeasonMmr`s by profile_uuid

## `SeasonMmr`
* profile_uuid: string
* season_uuid: string
* tier: MmrTier
* games_count: int
* wins_count: int
* wins: MmrTier[]

## `MmrTier`
* tier: int
  * number representation of competitive tier (0 = unranked, 1 & 2 unused, 3 = Iron 1, 4 = Iron 2, ..., 27 = Radiant)
* tier_name: string
  * string representation of competitive tier (e.g. Gold 3)


## `Season`
* season_uuid: string
* display_name: string
* start_time: timestamp
* end_time: timestamp

---
## `MatchMmr`
* match_uuid: string
* profile_uuid: string
* tier: MmrTier
* rr_in_tier: int
* total_rr: int
* rr_delta: int

* one to many `Match`s
* many to one `Profile`

## `Match`
* match_uuid: string
* start_time: timestamp
* match_length_seconds: int
* version: string
* map: Map
* mode: string
  * should always be competitive
* season_uuid: string
* server_cluster: string

## `MatchPlayer`
* match_uuid: string
* profile_uuid: string
* party_uuid: string
* player_card_uuid: string
* player_title_uuid: string
* player_level: int
* team_name: string
  * red/blue (see MatchTeam)
* agent: Agent
* tier: MmrTier
* session_time_milliseconds: int

* many to one `Match`

## `MatchTeam`
* team_name: string
  * red or blue
* match_uuid: string
* starting_side: string
  * offense or defense
* has_won: int
* rounds_won: int
* rounds_lost: int

* many to one `Match`

## `MatchPlayerStats`
* match_uuid: string
* profile_uuid: string
* c_casts_count: int
* q_casts_count: int
* e_casts_count: int
* x_casts_count: int
* kills_total: int
* assists_total: int
* deaths_total: int
* headshots_total: int
* bodyshots_total: int
* legshots_total: int
* score_total: int
* damage_outgoing_total: int
* damage_incoming_total: int
* economy_spend_total: int
* economy_spend_average: int
* loadout_value_total: int
* loadout_value_average: int

* many to one `Match`

## `MatchRound`
* match_uuid: string
* round_number: int
* winning_team_name: string
* end_type: string
  * eliminated/detonated/defused/expired
* bomb_planted: boolean
* bomb_defused: boolean

* many to one `Match`

## `MatchRoundSpikeEvent`
* match_uuid: string
* round_number: int
* profile_uuid: string
* location_x: int
* location_y: int
* round_time_milliseconds: int
* type: string
  * plant/defuse
* site: string
  * A/B/C (only applies to plant events)
* player_locations: { profile_uuid: string, location_x: int, location_y: int, view_radians: float }[]

* many to one `MatchRound`

## `MatchRoundPlayerStats`
* match_uuid: string
* round_number: int
* profile_uuid: string
* c_casts_count: int
* q_casts_count: int
* e_casts_count: int
* x_casts_count: int
* kills: int
* assists: int
* deaths: int
* headshots: int
* bodyshots: int
* legshots: int
* score: int
* weapon_uuid: string
* armor_uuid: string
* economy_spend: int
* economy_remaining: int
* loadout_value: int

* many to one `MatchRound`

## `MatchRoundKillEvent`
* match_uuid: string
* round_number: int
* killer_profile_uuid: string
* victim_profile_uuid: string
* victim_location_x: int
* victim_location_y: int
* assistant_profile_uuids: string[]
* player_locations: { profile_uuid: string, location_x: int, location_y: int, view_radians: float }[]
* weapon_uuid: string
* secondary_fire: boolean
* agent_ability_uuid: string
* round_time_milliseconds: int
* match_time_milliseconds: int

* many to one `Match`, `MatchRound`