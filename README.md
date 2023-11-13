# classdaigram1
abstract class Autoscaler {
    - EventListener
    - Scaler
    + dependency(EventListener)
    + triggerScalingActions()
}

class EventListener {
    + trigger(RulesEngine)
    + trigger(MetricsAPI)
}

class Scaler {
    - RulesEngine
    + triggerScalingActions(ReplicaSets, Pods)
}

class KubernetesComponents {
    - Pods
    - ReplicaSets
    - Nodes
}

class MetricCollector {
    - MetricsAPI
    - CustomMetricsAPI
    + collectMetrics(Pods, Nodes)
}

class ScalerRules {
    - RulesEngine
    - ScalingRules
    + evaluateMetrics()
}

class Notifier {
    - NotificationService
    + triggerNotifications(ScalingActions, Events, Alerts)
}

Autoscaler "uses" *-- "dependency" EventListener
EventListener "triggers" *-- "MetricsAPI" MetricCollector
EventListener "triggers" *-- "RulesEngine" ScalerRules
MetricCollector "collects" *-- "Pods" : Collects metrics from
MetricCollector "collects" *-- "Nodes" : Collects metrics from
RulesEngine "evaluates" *-- "ScalingRules"
Scaler "uses" *-- "RulesEngine" : Uses results to trigger
Scaler "uses" *-- "Pods" : Uses to trigger
Scaler "uses" *-- "ReplicaSets" : Uses to trigger
Scaler "triggers" *-- "NotificationService" Notifier
