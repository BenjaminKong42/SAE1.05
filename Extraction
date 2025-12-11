from datetime import datetime




def parse_datetime(dt):
    year = dt[0:4]
    month = dt[4:6]
    day = dt[6:8]
    hour = dt[9:11]
    minute = dt[11:13]
    return f"{day}-{month}-{year}", f"{hour}:{minute}"


def duration(start, end):
    fmt = "%Y%m%dT%H%M%S"
    s = datetime.strptime(start[:-1], fmt)
    e = datetime.strptime(end[:-1], fmt)
    diff = e - s
    h = diff.seconds // 3600
    m = (diff.seconds % 3600) // 60
    return f"{h:02d}:{m:02d}"


def extract_events(path):
    events = []
    with open(path, "r", encoding="utf-8") as f:
        lines = [l.strip() for l in f.readlines()]

    current = {}
    inside = False

    for line in lines:

        if line == "BEGIN:VEVENT":
            inside = True
            current = {}
            continue

        if line == "END:VEVENT":
            uid = current.get("UID", "vide")
            start = current.get("DTSTART")
            end = current.get("DTEND")

            date, heure = parse_datetime(start)
            duree = duration(start, end)

            summary = current.get("SUMMARY", "vide")
            location = current.get("LOCATION", "vide")
            description = current.get("DESCRIPTION", "")

            salles = "|".join(location.split(",")) if location != "vide" else "vide"

            desc_lines = [l for l in description.split("\\n") if l]
            profs = [l for l in desc_lines if "RT" not in l]
            groupes = [l for l in desc_lines if "RT" in l]

            profs = "|".join(profs) if profs else "vide"
            groupes = "|".join(groupes) if groupes else "vide"

            modalite = "CM"
            if "TP" in summary:
                modalite = "TP"
            elif "TD" in summary:
                modalite = "TD"
            elif "DS" in summary:
                modalite = "DS"

            csv_line = f"{uid};{date};{heure};{duree};{modalite};{summary};{salles};{profs};{groupes}"

            events.append(csv_line)
            inside = False
            continue

        if inside and ":" in line:
            key, value = line.split(":", 1)
            current[key] = value

    return events



if __name__ == "__main__":
    fichier = "ADE_RT1_Septembre2025_Decembre2025.ics"
    evt = extract_events(fichier)

    for e in evt[:10]:
        print(e)




