# Enum 使用实例

```rust
use chrono::{DateTime, Utc};

#[derive(Debug)]
pub enum UserStatus {
    Active,
    Inactive,
    Suspended { until: DateTime<Utc> },
    Deleted { deleted_at: DataTime<Utc> },
}

impl UserStatus {
    fn suspend(&mut self, until: DateTime<Utc>) {
        match self {
            UserStatus::Active => *self = 
                UserStatus::Suspended { until },
            _ => {}
        }
    }

    fn activate(&mut self) -> Result<(), &'static str> {
        match self {
            UserStates::Deleted { .. } => 
                return Err("cant't activate a deleted user"),
            _ => *self = UserStates::Active
        }
        Ok(())
    }

    fn delete(&mut self) {
        if let UserStates::Deleted { .. } = self {
            return;
        }
        *self = UserStatus::Deleted {
            deleted_at: Utc::now(),
        }
    }

    fn is_active(&self) -> bool {
        matches!(self, UserStatus::Active)
    }

    fn is_suspended(&self) -> bool {
        matches!(self, UserStatus::Suspended { .. })
    }

    fn is_deleted(&self) -> bool {
        matches!(self, UserStatus::Deleted { .. })
    }
}

#[cfg(test)]
mod test {
    use chrono::Duration;
    use super::*;

    #[test]
    fn test_user_status() -> Result<(), &'static str> {
        let mut status = UserStatus::Active;
        assert!(status.is_active());
        // Suspend until tomorrow
        status.suspend(Utc::now() + Duration::days(1));
        assert!(status.is_suspended());
        status.activate()?;
        assert!(status.is_active());
        status.delete();
        assert!(status.is_deleted());
        Ok(())
    }

    #[test]
    fn test_user_status_transition() {
        let mut status = UserStatus::Active;
        assert!(status.is_active());
        status.delete();
        assert!(status.is_deleted());
        // Can't activate a deleted user
        assert!(status.activate().is_err());
    }
}
```

## reference

[Using Enums to Represent State | Corrode Rust Consulting](https://corrode.dev/blog/enums/)